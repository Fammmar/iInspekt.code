using System;
using System.Data;
using System.Drawing;
using System.Windows.Forms;
namespace Parking
{
    public partial class MainForm : Form
    {
        private DatabaseHelper dbHelper;
        private DataGridView dgvParkingSpots;
        private Button btnLoadSpots;
        private Button btnOpenParkingForm;

        public MainForm()
        {
            InitializeMyComponents();
            dbHelper = new DatabaseHelper();
            LoadParkingSpots();
        }

        private void InitializeMyComponents()
        {
            this.Text = "Система управления парковкой";
            this.Size = new Size(900, 600);
            this.StartPosition = FormStartPosition.CenterScreen;
            this.BackColor = Color.White;

            // читаемость: нет группировки инициализации контролов
            // читаемость: размеры и координаты заданы магическими числами
            // улучшение архитектуры: вынести настройки стилей в отдельный метод или класс theme
            var lblTitle = new Label();
            lblTitle.Text = "Обзор парковочных мест";
            lblTitle.Font = new Font("Arial", 16, FontStyle.Bold);
            lblTitle.Location = new Point(20, 15);
            lblTitle.Size = new Size(300, 30);
            lblTitle.ForeColor = Color.DarkBlue;

            btnLoadSpots = new Button();
            btnLoadSpots.Text = "Обновить статус";
            btnLoadSpots.Location = new Point(20, 60);
            btnLoadSpots.Size = new Size(120, 35);
            btnLoadSpots.BackColor = Color.LightBlue;
            btnLoadSpots.Click += BtnLoadSpots_Click;

            btnOpenParkingForm = new Button();
            btnOpenParkingForm.Text = "Управление парковкой";
            btnOpenParkingForm.Location = new Point(150, 60);
            btnOpenParkingForm.Size = new Size(150, 35);
            btnOpenParkingForm.BackColor = Color.LightGreen;
            btnOpenParkingForm.Click += BtnOpenParkingForm_Click;

            dgvParkingSpots = new DataGridView();
            dgvParkingSpots.Location = new Point(20, 110);
            dgvParkingSpots.Size = new Size(840, 400);
            dgvParkingSpots.ReadOnly = true;
            dgvParkingSpots.AutoSizeColumnsMode = DataGridViewAutoSizeColumnsMode.Fill;
            dgvParkingSpots.BackgroundColor = Color.White;
            dgvParkingSpots.RowHeadersVisible = false;
            // возможная ошибка: не заданы имена колонок вручную, привязка идет по индексу в loadparkingspots
            // эффективность: autosizecolumnsmode.fill может тормозить при большом количестве строк

            var statusLabel = new Label();
            statusLabel.Text = "Для управления въездом/выездом нажмите 'Управление парковкой'";
            statusLabel.Location = new Point(20, 520);
            statusLabel.Size = new Size(500, 20);
            statusLabel.ForeColor = Color.Gray;

            this.Controls.AddRange(new Control[] {
                lblTitle,
                btnLoadSpots,
                btnOpenParkingForm,
                dgvParkingSpots,
                statusLabel
            });
            
            // улучшение архитектуры: нет обработки изменения размера окна
            // возможная ошибка: если окно уменьшить, часть контролов станет невидимой
        }

        private void BtnLoadSpots_Click(object sender, EventArgs e)
        {
            LoadParkingSpots();
            // оптимизация: можно было бы вызвать LoadParkingSpots напрямую в обработчике, 
            // но отдельный метод позволяет переиспользовать логику
        }

        private void BtnOpenParkingForm_Click(object sender, EventArgs e)
        {
            ParkingForm parkingForm = new ParkingForm();
            parkingForm.Show();
            // возможная ошибка: форма не закрывается при закрытии главной, может висеть в памяти
            // улучшение архитектуры: добавить parkingForm.Owner = this для привязки к главной форме
            // возможная ошибка: если нажать кнопку несколько раз, откроется много копий формы
        }

        private void LoadParkingSpots()
        {
            try
            {
                // читаемость: sql-запрос в коде, сложно поддерживать и читать
                // улучшение архитектуры: вынести запрос в хранимую процедуру или отдельный файл ресурсов
                // возможная ошибка: sql-инъекция невозможна, так как нет параметров, но запрос жестко зашит
                // возможная ошибка: SUBSTRING(SpotNumber, 2, 10) упадет если номер места короче 2 символов
                // возможная ошибка: CAST(SUBSTRING...) AS INT упадет если в номере есть буквы после первой
                string query = @"
            SELECT 
                SpotNumber,
                SpotType,
                IsOccupied,
                HourlyRate
            FROM ParkingSpots 
            ORDER BY 
                CASE 
                    WHEN LEFT(SpotNumber, 1) = 'A' THEN 1
                    WHEN LEFT(SpotNumber, 1) = 'B' THEN 2
                    WHEN LEFT(SpotNumber, 1) = 'C' THEN 3
                    WHEN LEFT(SpotNumber, 1) = 'D' THEN 4
                    ELSE 5
                END,
                CAST(SUBSTRING(SpotNumber, 2, 10) AS INT)";

                DataTable spots = dbHelper.ExecuteQuery(query);

                // читаемость: создание новой таблицы каждый раз при обновлении
                // оптимизация: можно использовать существующую таблицу и менять только данные
                DataTable displayTable = new DataTable();
                displayTable.Columns.Add("Номер места");
                displayTable.Columns.Add("Тип места");
                displayTable.Columns.Add("Статус");
                displayTable.Columns.Add("Тариф в час");

                foreach (DataRow row in spots.Rows)
                {
                    // возможная ошибка: Convert.ToBoolean может выбросить исключение если значение не bool
                    bool isOccupied = Convert.ToBoolean(row["IsOccupied"]);
                    string status = isOccupied ? "Занято" : "Свободно";

                    // возможная ошибка: если SpotType не Standard и не VIP, то russianSpotType будет пустым
                    string spotType = row["SpotType"].ToString();
                    string russianSpotType = spotType == "Standard" ? "Стандарт" : "VIP";
                    
                    // возможная ошибка: HourlyRate может быть null, ToString() не упадет, но даст пустую строку
                    displayTable.Rows.Add(
                        row["SpotNumber"].ToString(),
                        russianSpotType,
                        status,
                        row["HourlyRate"]
                    );
                }

                dgvParkingSpots.DataSource = displayTable;

                // эффективность: проход по всем строкам после привязки данных
                // оптимизация: можно задать стили через событие CellFormatting вместо цикла
                foreach (DataGridViewRow gridRow in dgvParkingSpots.Rows)
                {
                    // возможная ошибка: если колонка Статус была переименована или удалена, обращение по имени упадет
                    if (gridRow.Cells["Статус"].Value?.ToString() == "Занято")
                    {
                        gridRow.DefaultCellStyle.BackColor = Color.LightCoral;
                        gridRow.DefaultCellStyle.ForeColor = Color.DarkRed;
                    }
                    else
                    {
                        gridRow.DefaultCellStyle.BackColor = Color.LightGreen;
                        gridRow.DefaultCellStyle.ForeColor = Color.DarkGreen;
                    }
                }
            }
            catch (Exception ex)
            {
                // читаемость: сообщение об ошибке не локализовано
                // улучшение архитектуры: добавить логирование ошибки
                // возможная ошибка: если ошибка в цикле окрашивания, таблица останется без данных
                MessageBox.Show($"Ошибка загрузки данных: {ex.Message}");
            }
        }
    }
}
