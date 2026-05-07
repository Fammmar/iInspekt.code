Комментирование кода на KOTLIN
------------------------------------------------------------------




    <!-- Строка с чекбоксом и названием задачи, горизонтальная -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину родителя хотя можно сделать на половину для удобства пользователя -->
        android:layout_height="wrap_content" <!-- По высоте содержимого -->
        android:orientation="horizontal" <!-- Элементы в одну строку, желательно столбиком -->
        android:gravity="center_vertical"> <!-- Выравнивание по вертикали по центру -->

        <!-- Чекбокс для отметки выполнения задачи -->
        <CheckBox
            android:id="@+id/checkbox" <!-- ID для доступа из кода (TaskAdapter) -->
            android:layout_width="wrap_content" <!-- Ширина по размеру самого чекбокса -->
            android:layout_height="wrap_content" <!-- Высота по содержимому -->
            android:buttonTint="#FD7500" <!-- Цвет галочки оранжевый, при изменении темы будет плохо виден -->
            android:layout_marginEnd="12dp" /> <!-- Отступ справа от чекбокса до текста что не очень удобно для большинства-->

        <!-- Текст названия задачи -->
        <TextView
            android:id="@+id/titleText" <!-- ID для доступа из кода -->
            android:layout_width="0dp" <!-- Ширина растягивается (работает с weight) что может создавать проблему при повороте экрана -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:layout_weight="1" <!-- Забирает всё свободное место в строке -->
            android:text="Задача" <!-- Текст по умолчанию (заглушка) -->
            android:textSize="20sp" <!-- Размер шрифта -->
            android:textColor="#FFFFFF" <!-- Белый цвет текста -->
            android:textStyle="bold" /> <!-- Жирное начертание -->

    </LinearLayout>

    <!-- Строка с датой и категорией, горизонтальная -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину -->
        android:layout_height="wrap_content" <!-- По содержимому -->
        android:orientation="horizontal" <!-- Горизонтально -->
        android:gravity="center_vertical" <!-- По центру вертикали -->
        android:layout_marginTop="4dp" <!-- Отступ сверху от заголовка -->
        android:layout_marginStart="32dp"> <!-- Отступ слева, чтобы выровнять под чекбоксом -->

        <!-- Текст даты и времени задачи -->
        <TextView
            android:id="@+id/dateTimeText" <!-- ID для доступа из адаптера -->
            android:layout_width="0dp" <!-- Растягивается -->
            android:layout_height="wrap_content" <!-- По тексту -->
            android:layout_weight="1" <!-- Занимает остаток места -->
            android:text="Завтра, 12:00" <!-- Текст-заглушка -->
            android:textSize="16sp" <!-- Размер шрифта -->
            android:textColor="#FAF6F6" /> <!-- Светлый цвет текста -->

        <!-- Текст категории (личное, работа, учёба и т.д.) -->
        <TextView
            android:id="@+id/categoryText" <!-- ID для доступа из кода (можно расширить) -->
            android:layout_width="wrap_content" <!-- Ширина по тексту -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:text="Личное" <!-- Текст-заглушка -->
            android:textSize="14sp" <!-- Размер шрифта -->
            android:textColor="#0A024B" <!-- Цвет текста совпадает с фоном → не видно! Нужно менять -->
            android:background="#0A024B" <!-- Фон такой же как у карточки -->
            android:paddingHorizontal="12dp" <!-- Горизонтальные отступы внутри -->
            android:paddingVertical="4dp" <!-- Вертикальные отступы внутри -->
            android:layout_marginStart="8dp" /> <!-- Отступ слева от даты -->

    </LinearLayout>

    <!-- Контейнер для подписи "Напомнить" и спиннера, горизонтальный -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину -->
        android:layout_height="wrap_content" <!-- По содержимому -->
        android:orientation="horizontal" <!-- В строку -->
        android:gravity="center_vertical" <!-- По центру вертикали -->
        android:layout_marginTop="8dp" <!-- Отступ сверху -->
        android:layout_marginStart="32dp"> <!-- Отступ слева 32dp -->

        <!-- Текст-метка "Напомнить:" -->
        <TextView
            android:layout_width="wrap_content" <!-- Ширина по тексту -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:layout_marginEnd="8dp" <!-- Отступ справа от текста до спиннера -->
            android:text="Напомнить:" <!-- Текст метки -->
            android:textColor="#695A9E" <!-- Цвет текста серо-фиолетовый -->
            android:textSize="12sp" /> <!-- Размер шрифта 12sp -->

        <!-- Выпадающий список для выбора времени напоминания -->
        <Spinner
            android:id="@+id/reminderSpinner" <!-- ID, используется в адаптере, но отключён (enabled=false) -->
            android:layout_width="195dp" <!-- Фиксированная ширина, не адаптивно -->
            android:layout_height="wrap_content" <!-- Высота по содержимому -->
            android:layout_weight="1" /> <!-- Вес для растягивания (но при фикс. ширине не работает) -->

    </LinearLayout>

    <!-- Выпадающий список для повторения задачи (без подписи) -->
    <Spinner
        android:id="@+id/repeatSpinner" <!-- ID, используется в адаптере, отключён -->
        android:layout_width="209dp" <!-- Фиксированная ширина -->
        android:layout_height="wrap_content" <!-- Высота по содержимому -->
        android:layout_weight="1" /> <!-- Вес для растягивания -->

    <!-- Контейнер для подписи "Повтор:" (отдельно от спиннера — неудобно) -->
    <LinearLayout
        android:layout_width="wrap_content" <!-- Ширина по содержимому -->
        android:layout_height="wrap_content" <!-- Высота по содержимому -->
        android:layout_marginStart="32dp" <!-- Отступ слева 32dp -->
        android:layout_marginTop="4dp" <!-- Отступ сверху -->
        android:gravity="center_vertical" <!-- По центру вертикали -->
        android:orientation="horizontal"> <!-- Горизонтально (хотя внутри один элемент) -->

        <!-- Текст-метка "Повтор:" -->
        <TextView
            android:layout_width="wrap_content" <!-- Ширина по тексту -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:layout_marginEnd="8dp" <!-- Отступ справа -->
            android:text="Повтор:" <!-- Текст метки -->
            android:textColor="#B0BEC5" <!-- Светлый серо-голубой цвет -->
            android:textSize="12sp" /> <!-- Мелкий шрифт -->

    </LinearLayout>

    <!-- Контейнер для текста остатка времени и прогресс-бара, вертикальный -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину -->
        android:layout_height="wrap_content" <!-- По содержимому -->
        android:orientation="vertical" <!-- Вертикально: текст сверху, полоса снизу -->
        android:layout_marginTop="12dp" <!-- Отступ сверху -->
        android:layout_marginStart="32dp"> <!-- Отступ слева 32dp -->

        <!-- Текст с информацией об оставшемся времени -->
        <TextView
            android:id="@+id/timeRemainingText" <!-- ID для доступа из адаптера -->
            android:layout_width="match_parent" <!-- На всю ширину контейнера -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:text="Осталось: 2 дня 5 часов" <!-- Текст-заглушка -->
            android:textSize="13sp" <!-- Размер шрифта -->
            android:textColor="#FD7500" <!-- Оранжевый цвет текста -->
            android:layout_marginBottom="6dp" /> <!-- Отступ снизу до прогресс-бара -->

        <!-- Полоса прогресса выполнения задачи -->
        <ProgressBar
            android:id="@+id/progressBar" <!-- ID для доступа из адаптера -->
            style="?android:attr/progressBarStyleHorizontal" <!-- Горизонтальный стиль прогресс-бара -->
            android:layout_width="match_parent" <!-- На всю ширину -->
            android:layout_height="wrap_content" <!-- Стандартная высота -->
            android:max="100" <!-- Максимальное значение 100% -->
            android:progress="60" <!-- Текущий прогресс 60% (заглушка) -->
            android:progressTint="#FCFD7700" /> <!-- Жёлтый цвет полосы прогресса -->
    </LinearLayout>

</LinearLayout>



------------------------------------------------------------------

разметка HTML

    <!-- Контейнер для чекбокса и заголовка, горизонтальное расположение -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину родителя -->
        android:layout_height="wrap_content" <!-- Высота по содержимому -->
        android:orientation="horizontal" <!-- Элементы в строку -->
        android:gravity="center_vertical"> <!-- Выравнивание по центру по вертикали -->

        <!-- Чекбокс — отмечает задачу как выполненную -->
        <CheckBox
            android:id="@+id/checkbox" <!-- Идентификатор для доступа из кода -->
            android:layout_width="wrap_content" <!-- Ширина по размеру самого элемента -->
            android:layout_height="wrap_content" <!-- Высота по содержимому -->
            android:buttonTint="#FD7500" <!-- Цвет галочки оранжевый -->
            android:layout_marginEnd="12dp" /> <!-- Отступ справа от чекбокса -->

        <!-- Текст заголовка задачи -->
        <TextView
            android:id="@+id/titleText" <!-- Идентификатор для доступа из кода -->
            android:layout_width="0dp" <!-- Занимает всё свободное место с учётом weight -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:layout_weight="1" <!-- Вес для растягивания -->
            android:text="Задача" <!-- Текст-заглушка -->
            android:textSize="20sp" <!-- Размер шрифта 20sp -->
            android:textColor="#FFFFFF" <!-- Белый цвет текста -->
            android:textStyle="bold" /> <!-- Жирное начертание -->

    </LinearLayout>

    <!-- Контейнер для даты и категории, горизонтальный -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину -->
        android:layout_height="wrap_content" <!-- По содержимому -->
        android:orientation="horizontal" <!-- В строку -->
        android:gravity="center_vertical" <!-- По центру вертикали -->
        android:layout_marginTop="4dp" <!-- Отступ сверху от предыдущего блока -->
        android:layout_marginStart="32dp"> <!-- Отступ слева 32dp (под чекбокс) -->

        <!-- Текст даты и времени задачи -->
        <TextView
            android:id="@+id/dateTimeText" <!-- Идентификатор -->
            android:layout_width="0dp" <!-- Растягивается -->
            android:layout_height="wrap_content" <!-- По тексту -->
            android:layout_weight="1" <!-- Занимает остаток места -->
            android:text="Завтра, 12:00" <!-- Текст-заглушка -->
            android:textSize="16sp" <!-- Размер шрифта -->
            android:textColor="#FAF6F6" /> <!-- Светло-серый цвет -->

        <!-- Текст категории (личное, работа и т.д.) -->
        <TextView
            android:id="@+id/categoryText" <!-- Идентификатор -->
            android:layout_width="wrap_content" <!-- Ширина по тексту -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:text="Личное" <!-- Текст-заглушка -->
            android:textSize="14sp" <!-- Размер шрифта -->
            android:textColor="#0A024B" <!-- Цвет текста такой же как фон → не видно -->
            android:background="#0A024B" <!-- Фон как у карточки -->
            android:paddingHorizontal="12dp" <!-- Горизонтальные внутренние отступы -->
            android:paddingVertical="4dp" <!-- Вертикальные внутренние отступы -->
            android:layout_marginStart="8dp" /> <!-- Отступ слева от даты -->

    </LinearLayout>

    <!-- Контейнер для подписи "Напомнить" и выпадающего списка -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину -->
        android:layout_height="wrap_content" <!-- По содержимому -->
        android:orientation="horizontal" <!-- В строку -->
        android:gravity="center_vertical" <!-- По центру вертикали -->
        android:layout_marginTop="8dp" <!-- Отступ сверху -->
        android:layout_marginStart="32dp"> <!-- Отступ слева 32dp -->

        <!-- Текст-метка "Напомнить:" -->
        <TextView
            android:layout_width="wrap_content" <!-- Ширина по тексту -->
            android:layout_height="wrap_content" <!-- Высота по тексту -->
            android:layout_marginEnd="8dp" <!-- Отступ справа от текста -->
            android:text="Напомнить:" <!-- Текст метки -->
            android:textColor="#695A9E" <!-- Серо-фиолетовый цвет -->
            android:textSize="12sp" /> <!-- Мелкий шрифт -->

        <!-- Выпадающий список для выбора времени напоминания -->
        <Spinner
            android:id="@+id/reminderSpinner" <!-- Идентификатор -->
            android:layout_width="195dp" <!-- Фиксированная ширина -->
            android:layout_height="wrap_content" <!-- Высота по содержимому -->
            android:layout_weight="1" /> <!-- Вес для растягивания -->

    </LinearLayout>

    <!-- Выпадающий список для выбора периодичности повторения задачи -->
    <Spinner
        android:id="@+id/repeatSpinner" <!-- Идентификатор -->
        android:layout_width="209dp" <!-- Фиксированная ширина -->
        android:layout_height="wrap_content" <!-- Высота по содержимому (было 1dp — исправлено) -->
        android:layout_weight="1" /> <!-- Вес для растягивания -->

    <!-- Контейнер для подписи "Повтор:" (отдельно от спиннера) -->
    <LinearLayout
        android:layout_width="wrap_content" <!-- Ширина по содержимому -->
        android:layout_height="wrap_content" <!-- Высота по содержимому -->
        android:layout_marginStart="32dp" <!-- Отступ слева 32dp -->
        android:layout_marginTop="4dp" <!-- Отступ сверху -->
        android:gravity="center_vertical" <!-- По центру вертикали -->
        android:orientation="horizontal"> <!-- Горизонтальное расположение (хотя внутри один элемент) -->

        <!-- Текст-метка "Повтор:" -->
        <TextView
            android:layout_width="wrap_content" <!-- Ширина по тексту -->
            android:layout_height="wrap_content" <!-- Высота по тексту (было 0dp — исправлено) -->
            android:layout_marginEnd="8dp" <!-- Отступ справа -->
            android:text="Повтор:" <!-- Текст метки -->
            android:textColor="#B0BEC5" <!-- Светлый серо-голубой -->
            android:textSize="12sp" /> <!-- Мелкий шрифт -->

    </LinearLayout>

    <!-- Контейнер для текста остатка времени и прогресс-бара -->
    <LinearLayout
        android:layout_width="match_parent" <!-- На всю ширину -->
        android:layout_height="wrap_content" <!-- По содержимому -->
        android:orientation="vertical" <!-- Вертикально (текст сверху, полоса снизу) -->
        android:layout_marginTop="12dp" <!-- Отступ сверху -->
        android:layout_marginStart="32dp"> <!-- Отступ слева 32dp -->

        <!-- Текст с информацией, сколько осталось времени до дедлайна -->
        <TextView
            android:id="@+id/timeRemainingText" <!-- Идентификатор -->
            android:layout_width="match_parent" <!-- На всю ширину -->
            android:layout_height="wrap_content" <!-- По тексту -->
            android:text="Осталось: 2 дня 5 часов" <!-- Текст-заглушка -->
            android:textSize="13sp" <!-- Размер шрифта -->
            android:textColor="#FD7500" <!-- Оранжевый цвет -->
            android:layout_marginBottom="6dp" /> <!-- Отступ снизу перед прогресс-баром -->

        <!-- Полоса прогресса выполнения задачи -->
        <ProgressBar
            android:id="@+id/progressBar" <!-- Идентификатор -->
            style="?android:attr/progressBarStyleHorizontal" <!-- Горизонтальный стиль -->
            android:layout_width="match_parent" <!-- На всю ширину -->
            android:layout_height="wrap_content" <!-- Высота стандартная -->
            android:max="100" <!-- Максимальное значение 100% -->
            android:progress="60" <!-- Текущий прогресс 60% (заглушка) -->
            android:progressTint="#FCFD7700" /> <!-- Жёлтый цвет полосы -->

    </LinearLayout>

</LinearLayout>
