<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shade Executor</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap" rel="stylesheet"> <!-- Подключаем шрифт Roboto -->
    <style>
        /* Основные стили для тела страницы */
        body {
            font-family: 'Roboto', sans-serif; /* Используем шрифт Roboto */
            background: linear-gradient(to bottom right, #1e1e2f, #000000); /* Градиентный фон */
            color: #ffffff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            position: relative;
            overflow: hidden;
            opacity: 0; /* Начальная непрозрачность для анимации загрузки */
            animation: fade-in 1.2s forwards; /* Анимация появления */
        }

        /* Анимация появления */
        @keyframes fade-in {
            to {
                opacity: 1; /* Конечная непрозрачность */
            }
        }

        /* Заголовок страницы */
        h1 {
            margin-bottom: 20px;
            font-size: 48px; /* Увеличенный размер */
            text-shadow: 3px 3px 10px rgba(0, 0, 0, 0.7);
            letter-spacing: 1.5px; /* Пробел между буквами */
            display: flex; /* Используем flexbox для выравнивания */
            align-items: center; /* Выравниваем по центру по вертикали */
            transition: transform 0.5s ease; /* Плавный переход для заголовка */
        }

        h1:hover {
            transform: scale(1.05); /* Увеличение заголовка при наведении */
        }

        /* Описание */
        p {
            margin-bottom: 40px;
            font-size: 20px; /* Увеличенный размер текста */
            text-align: center;
            max-width: 600px;
            text-shadow: 1px 1px 5px rgba(0, 0, 0, 0.5);
            transition: color 0.3s ease; /* Плавный переход цвета для текста */
        }

        p:hover {
            color: #a8dadc; /* Изменение цвета текста при наведении */
        }

        /* Стили кнопок */
        .button {
            padding: 15px 30px;
            margin: 10px;
            border: 3px solid #a8dadc; /* Видимая обводка */
            border-radius: 5px;
            cursor: pointer;
            font-size: 18px;
            background-color: #2e4053; /* Темно-синий цвет кнопки */
            color: white;
            font-weight: bold;
            transition: transform 0.3s ease, box-shadow 0.3s ease, background-color 0.3s ease;
            position: relative;
            overflow: hidden;
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.5); /* Тень для кнопок */
        }

        .button:hover {
            transform: translateY(-3px); /* Подъем кнопки при наведении */
            box-shadow: 0 10px 20px rgba(255, 255, 255, 0.5);
            background-color: rgba(255, 255, 255, 0.1); /* Светлый фон при наведении */
        }

        /* Эффект обводки кнопок */
        .button::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(255, 255, 255, 0.1);
            z-index: 0;
            border-radius: 5px;
            transition: background 0.3s ease; /* Плавный переход для фона кнопки */
        }

        /* Стили для текста на кнопках */
        .button span {
            position: relative;
            z-index: 1;
            padding-left: 30px; /* Отступ для текста от иконки */
        }

        /* Переливающаяся обводка для кнопок */
        .button:hover {
            animation: border-animation 1.5s infinite linear;
        }

        @keyframes border-animation {
            0% { 
                border-color: #a8dadc; /* Исходный цвет обводки */
                box-shadow: 0 0 5px #a8dadc; 
            }
            14% {
                box-shadow: 0 0 5px #457b9d; /* Синий */
            }
            28% {
                box-shadow: 0 0 5px #1d3557; /* Темно-синий */
            }
            42% {
                box-shadow: 0 0 5px #f1faee; /* Белый */
            }
            57% {
                box-shadow: 0 0 5px #f1faee; /* Белый */
            }
            71% {
                box-shadow: 0 0 5px #457b9d; /* Синий */
            }
            85% {
                box-shadow: 0 0 5px #a8dadc; /* Светло-голубой */
            }
            100% {
                box-shadow: 0 0 5px #a8dadc; /* Светло-голубой */
            }
        }

        /* Фоновый эффект */
        .background-effect {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle, rgba(255, 255, 255, 0.05) 20%, rgba(255, 255, 255, 0) 100%);
            animation: pulse 4s infinite;
            z-index: -1;
        }

        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.05);
            }
            100% {
                transform: scale(1);
            }
        }

        /* Раздел с дополнительной информацией */
        .info {
            margin-top: 40px;
            font-size: 16px;
            text-align: center;
            max-width: 600px;
        }

        .info a {
            color: #00aaff;
            text-decoration: none;
            transition: color 0.3s ease; /* Плавный переход цвета */
        }

        .info a:hover {
            text-decoration: underline;
            color: #f1faee; /* Изменение цвета при наведении */
        }

        /* Иконки на кнопках */
        .button::before {
            content: '';
            position: absolute;
            left: 10px; /* Отступ слева */
            top: 50%;
            transform: translateY(-50%);
            width: 24px; /* Ширина иконки */
            height: 24px; /* Высота иконки */
            background-size: cover; /* Обложка для иконки */
        }

        .get-key::before {
            background-image: url('https://img.icons8.com/ios-filled/50/ffffff/snowflake.png'); /* Иконка "Снежинка" для кнопки "Получить Ключ" */
        }

        .download::before {
            background-image: url('https://img.icons8.com/ios-filled/50/ffffff/download.png'); /* Иконка "Скачать" */
        }

        /* Зимний фон */
        body::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: url('https://cdn.pixabay.com/photo/2016/11/29/09/21/snow-1867053_1280.jpg'); /* Фон со снегом */
            background-size: cover;
            opacity: 0.2; /* Полупрозрачный фон */
            z-index: -2;
        }
    </style>
</head>
<body>

    <div class="background-effect"></div>
    <h1>
        forever bloom
    </h1>
    <p>FB - это мощное решение для управления вашими скриптами. Получите доступ к уникальному читу, который поможет вам достичь успеха в вашей цели.</p>
    
    <button class="button get-key" onclick="window.open('https://t.me/kiopiZXC/13', '_blank');">
        <span>Получить Ключ</span>
    </button>
    
    <button class="button download" onclick="window.open('https://t.me/kiopiZXC/14', '_blank');">
        <span>Скачать</span>
    </button>

    <div class="info">
        <p>Для получения дополнительной информации и поддержки, пожалуйста, посетите <a href="#">наш тгк</a>.</p>
    </div>

</body>
</html>
