<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Специальный квест для моего зайчика</title>
    
    <!-- Мета-теги для iOS -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="format-detection" content="telephone=no">
    <meta name="theme-color" content="#EDEDE9">
    
    <!-- Иконки для iOS -->
    <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🐰</text></svg>">
    
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400;700&family=Roboto:wght@300;400;500&display=swap">
    <style>
        /* Сброс стилей и базовая настройка */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            user-select: none;
        }

        input, textarea, select {
            -webkit-user-select: auto;
            user-select: auto;
        }

        :root {
            --cream: #EDEDE9;
            --light-tan: #D6CCC2;
            --light-beige: #F5EBE0;
            --tan: #E3D5CA;
            --dark-tan: #D5BDAF;
            --text-dark: #5D534A;
            --accent: #9D6B53;
            --safe-top: env(safe-area-inset-top, 0px);
            --safe-bottom: env(safe-area-inset-bottom, 0px);
            --safe-left: env(safe-area-inset-left, 0px);
            --safe-right: env(safe-area-inset-right, 0px);
        }

        html, body {
            height: 100%;
            width: 100%;
            overflow: hidden;
            -webkit-text-size-adjust: 100%;
            touch-action: manipulation;
        }

        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--cream);
            color: var(--text-dark);
            line-height: 1.6;
            background-image: linear-gradient(to bottom, var(--cream) 0%, var(--light-beige) 100%);
            padding: var(--safe-top) var(--safe-right) var(--safe-bottom) var(--safe-left);
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
        }

        /* Контейнер для страниц */
        .pages-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
        }

        .page {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            padding: max(1rem, var(--safe-top)) max(1rem, var(--safe-right)) max(1rem, var(--safe-bottom)) max(1rem, var(--safe-left));
            opacity: 0;
            transform: translateX(100%);
            transition: transform 0.5s ease, opacity 0.5s ease;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .page.active {
            opacity: 1;
            transform: translateX(0);
            z-index: 10;
        }

        .page.previous {
            transform: translateX(-100%);
        }

        .page-content {
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 2rem 1.5rem;
            width: 100%;
            max-width: 800px;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.5);
            position: relative;
            margin: auto;
        }

        .page-content::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 8px;
            background: linear-gradient(to right, var(--dark-tan), var(--accent));
        }

        /* Типография */
        h1, h2 {
            font-family: 'Dancing Script', cursive;
            color: var(--accent);
            margin-bottom: 1.5rem;
            text-align: center;
            word-wrap: break-word;
        }

        h1 {
            font-size: clamp(2.2rem, 8vw, 3.5rem);
            margin-bottom: 1.5rem;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
        }

        h2 {
            font-size: clamp(1.8rem, 6vw, 2.8rem);
            margin-bottom: 1.2rem;
        }

        h3 {
            font-size: 1.4rem;
            color: var(--accent);
            margin-bottom: 1rem;
        }

        p {
            margin-bottom: 1.2rem;
            font-size: 1.1rem;
            line-height: 1.5;
        }

        .highlight {
            background-color: rgba(213, 189, 175, 0.3);
            padding: 0.2rem 0.5rem;
            border-radius: 5px;
            font-weight: 500;
        }

        .pet-name {
            color: var(--accent);
            font-weight: bold;
            font-style: italic;
            font-size: 1.2rem;
        }

        /* Компоненты */
        .task-container {
            background-color: var(--light-beige);
            padding: 1.2rem;
            border-radius: 15px;
            margin: 1.5rem 0;
            border: 1px dashed var(--tan);
        }

        .code-block {
            background-color: var(--cream);
            padding: 1.2rem;
            border-radius: 10px;
            font-family: monospace;
            font-size: 1rem;
            margin: 1.2rem 0;
            border-left: 5px solid var(--dark-tan);
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
        }

        /* Формы и кнопки - ОСНОВНОЕ ИСПРАВЛЕНИЕ ДЛЯ iOS */
        .input-container {
            display: flex;
            flex-direction: column;
            gap: 1rem;
            margin-top: 1.5rem;
        }

        input[type="text"],
        input[type="number"],
        select {
            width: 100%;
            padding: 16px; /* Больше padding для удобства на мобильных */
            border: 2px solid var(--tan);
            border-radius: 12px;
            font-size: 16px !important; /* Фиксированный размер шрифта для iOS */
            background-color: white;
            -webkit-appearance: none;
            appearance: none;
            min-height: 56px; /* Высота для удобного тапа */
            margin: 0;
        }

        /* Особые стили для iOS */
        @supports (-webkit-touch-callout: none) {
            input[type="text"],
            input[type="number"],
            select {
                font-size: 16px !important;
                line-height: 1.4 !important;
            }
            
            select {
                background-image: url('data:image/svg+xml;charset=utf-8,<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="%239D6B53"><path d="M7 10l5 5 5-5z"/></svg>');
                background-repeat: no-repeat;
                background-position: right 16px center;
                padding-right: 50px;
            }
        }

        input:focus,
        select:focus {
            outline: none;
            border-color: var(--accent);
            box-shadow: 0 0 0 3px rgba(157, 107, 83, 0.1);
        }

        /* КНОПКИ - ГЛАВНОЕ ИСПРАВЛЕНИЕ */
        button {
            background-color: var(--accent);
            color: white;
            border: none;
            padding: 16px 32px;
            border-radius: 12px;
            font-size: 1.1rem;
            font-weight: 500;
            cursor: pointer;
            min-height: 56px;
            width: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
            transition: all 0.2s ease;
            position: relative;
            overflow: hidden;
        }

        /* Активное состояние кнопки */
        button:active {
            background-color: var(--dark-tan);
            transform: scale(0.98);
        }

        /* Для десктопов */
        @media (hover: hover) {
            button:hover:not(:active) {
                background-color: var(--dark-tan);
                transform: translateY(-2px);
            }
        }

        button:disabled {
            background-color: var(--light-tan);
            cursor: not-allowed;
            transform: none !important;
        }

        /* Анимации */
        .heart {
            color: #e63946;
            display: inline-block;
            animation: heartbeat 1.5s infinite;
        }

        @keyframes heartbeat {
            0% { transform: scale(1); }
            5% { transform: scale(1.1); }
            10% { transform: scale(1); }
            15% { transform: scale(1.1); }
            20% { transform: scale(1); }
            100% { transform: scale(1); }
        }

        .decoration {
            text-align: center;
            margin: 1.5rem 0;
        }

        .decoration span {
            font-size: 2.2rem;
            margin: 0 0.3rem;
            color: var(--dark-tan);
        }

        .message-box {
            background-color: var(--light-beige);
            padding: 1.8rem;
            border-radius: 15px;
            margin: 1.5rem 0;
            text-align: center;
            border: 2px solid var(--tan);
        }

        .final-message {
            font-family: 'Dancing Script', cursive;
            font-size: clamp(1.6rem, 5vw, 2.2rem);
            line-height: 1.4;
            color: var(--accent);
            margin-bottom: 1.5rem;
        }

        .instructions {
            background-color: rgba(229, 213, 202, 0.3);
            padding: 1.2rem;
            border-radius: 10px;
            margin: 1.2rem 0;
            font-size: 1.1rem;
            border-left: 4px solid var(--dark-tan);
        }

        .task-counter {
            display: inline-block;
            background-color: var(--accent);
            color: white;
            padding: 0.4rem 1.2rem;
            border-radius: 20px;
            font-size: 0.9rem;
            margin-bottom: 1rem;
            font-weight: 500;
        }

        .progress-container {
            margin: 1.5rem 0;
            height: 10px;
            background-color: var(--light-tan);
            border-radius: 5px;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background: linear-gradient(to right, var(--dark-tan), var(--accent));
            width: 0%;
            transition: width 0.5s ease;
        }

        .error-message {
            color: #e63946;
            margin-top: 0.5rem;
            font-weight: 500;
            display: none;
            font-size: 0.95rem;
        }

        .success-message {
            color: var(--accent);
            margin-top: 0.5rem;
            font-weight: 500;
            display: none;
            font-size: 0.95rem;
        }

        .hint {
            color: var(--accent);
            font-style: italic;
            margin-top: 0.5rem;
            font-size: 0.9rem;
            opacity: 0.8;
        }

        .welcome-message {
            text-align: center;
            padding: 1.2rem;
            background: linear-gradient(135deg, rgba(245, 235, 224, 0.7), rgba(227, 213, 202, 0.7));
            border-radius: 15px;
            margin: 1.5rem 0;
        }

        .flower-icon {
            color: #e63946;
            font-size: 1.2rem;
            margin: 0 0.2rem;
        }

        .math-task {
            background-color: rgba(229, 213, 202, 0.5);
            padding: 1.2rem;
            border-radius: 10px;
            margin: 1rem 0;
        }

        .math-task ul {
            margin-left: 1.2rem;
            margin-bottom: 1rem;
        }

        .math-task li {
            margin-bottom: 0.5rem;
        }

        /* Конфетти */
        .confetti {
            position: fixed;
            width: 10px;
            height: 10px;
            pointer-events: none;
            z-index: 1000;
        }

        /* Адаптивность */
        @media (max-width: 768px) {
            .page-content {
                padding: 1.5rem 1rem;
                max-height: 85vh;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            h2 {
                font-size: 1.7rem;
            }
            
            button {
                padding: 14px 24px;
                min-height: 52px;
            }
        }

        /* Ландшафтный режим */
        @media (orientation: landscape) and (max-height: 500px) {
            .page-content {
                padding: 1rem;
                max-height: 80vh;
            }
            
            h1 {
                font-size: 1.8rem;
                margin-bottom: 0.8rem;
            }
            
            h2 {
                font-size: 1.5rem;
            }
            
            .task-container {
                padding: 1rem;
                margin: 1rem 0;
            }
        }

        /* Улучшение скролла для iOS */
        .page-content {
            scroll-behavior: smooth;
        }
        
        .page-content::-webkit-scrollbar {
            width: 0;
            background: transparent;
        }
    </style>
</head>
<body>
    <div class="pages-container">
        <!-- Страница 1: Приветствие -->
        <div id="page1" class="page active">
            <div class="page-content">
                <div class="decoration">
                    <span>❦</span><span>✽</span><span>❦</span>
                </div>
                
                <h1>Привет, мой зайчик!</h1>
                
                <div class="welcome-message">
                    <p>Да-да, именно для тебя, мой самый любимый зайчик на свете!</p>
                    <p>Я приготовил для тебя небольшой квест с сюрпризом в конце <span class="heart">♥</span></p>
                </div>
                
                <p>Ты же знаешь, что я программист, поэтому вместо обычного приглашения я создал для тебя этот небольшой сайт.</p>
                
                <div class="instructions">
                    <p><span class="highlight">Как жена программиста</span>, ты легко справишься с этими простыми заданиями!</p>
                    <p>Не волнуйся, всё будет очень просто, а я всегда готов помочь, если что-то пойдет не так.</p>
                </div>
                
                <p>Когда ты выполнишь все задания, тебя ждет приятный сюрприз, обещаю!</p>
                
                <div class="decoration">
                    <span>✽</span><span>❧</span><span>✽</span>
                </div>
                
                <div class="task-container" style="text-align: center;">
                    <h3>Готовы начать, зайчик?</h3>
                    <p>Жми на кнопку, чтобы перейти к первому заданию!</p>
                    
                    <div style="margin-top: 1.5rem;">
                        <button id="start-btn" class="tap-btn">Начать квест!</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Страница 2: Задание 1 -->
        <div id="page2" class="page">
            <div class="page-content">
                <div class="task-counter">Задание 1 из 4</div>
                <h2>Первое задание</h2>
                
                <p>Привет, <span class="pet-name">зайчик</span>! Как <span class="highlight">жена программиста</span>, ты должна знать кое-что о логике.</p>
                
                <div class="task-container">
                    <h3>Логическая задачка</h3>
                    <p>У программиста есть два яблока. Одно он отдает тебе, другое - оставляет себе. Сколько яблок осталось у программиста?</p>
                    
                    <div class="input-container">
                        <input type="number" id="apple-input" placeholder="Введи число" inputmode="numeric" min="0" max="10">
                        <button id="apple-btn" class="tap-btn">Проверить ответ</button>
                    </div>
                    <div class="error-message" id="apple-error">Неверный ответ, попробуй еще раз!</div>
                    <div class="success-message" id="apple-success">Правильно! Переходим дальше!</div>
                    <div class="hint">Подсказка: подумай, что сделал программист с яблоками</div>
                </div>
                
                <div class="progress-container">
                    <div class="progress-bar" id="progress-bar-1"></div>
                </div>
            </div>
        </div>

        <!-- Страница 3: Задание 2 -->
        <div id="page3" class="page">
            <div class="page-content">
                <div class="task-counter">Задание 2 из 4</div>
                <h2>Второе задание</h2>
                
                <p>Отлично справилась, <span class="pet-name">зайчик</span>! Теперь небольшое задание на внимательность.</p>
                
                <div class="task-container">
                    <h3>Найди ошибку в коде</h3>
                    <p>Программист написал код для признания в любви, но допустил ошибку. Помоги ему исправить её:</p>
                    
                    <div class="code-block">
function признаниеВЛюбви(имя) {
    let сообщение = "Я тебя люблю, " + имя + "!";
    console.log(сообщение);
    return сообщение;
}

// Вызов функции
признаниеВЛюбви(<span style="background-color: #ffebee; padding: 0 3px;">"моя любимая"</span>);
                    </div>
                    
                    <p>Программист хотел обратиться к тебе по твоему самому любимому прозвищу. Какое слово должно быть в кавычках вместо "моя любимая"?</p>
                    
                    <div class="input-container">
                        <input type="text" id="code-input" placeholder="Введи ласковое прозвище" autocapitalize="none">
                        <button id="code-btn" class="tap-btn">Проверить ответ</button>
                    </div>
                    <div class="error-message" id="code-error">Неверно, подумай еще! Как я тебя называю?</div>
                    <div class="success-message" id="code-success">Верно! Ты действительно мой зайчик!</div>
                    <div class="hint">Подсказка: я всегда называю тебя ласково, особенно когда пишу код с мыслями о тебе</div>
                </div>
                
                <div class="progress-container">
                    <div class="progress-bar" id="progress-bar-2"></div>
                </div>
            </div>
        </div>

        <!-- Страница 4: Задание 3 -->
        <div id="page4" class="page">
            <div class="page-content">
                <div class="task-counter">Задание 3 из 4</div>
                <h2>Третье задание</h2>
                
                <p>Ты просто умница, <span class="pet-name">зайчик</span>! Осталось совсем немного.</p>
                
                <div class="task-container">
                    <h3>Вопрос о нашем знакомстве</h3>
                    <p>Помнишь, когда мы впервые встретились? Я тогда еще не знал, что встретил свою будущую <span class="highlight">жену программиста</span>.</p>
                    <p>В каком месяце мы начали встречаться?</p>
                    
                    <div class="input-container">
                        <select id="month-select">
                            <option value="">Выбери месяц</option>
                            <option value="январь">Январь</option>
                            <option value="февраль">Февраль</option>
                            <option value="март">Март</option>
                            <option value="апрель">Апрель</option>
                            <option value="май">Май</option>
                            <option value="июнь">Июнь</option>
                            <option value="июль">Июль</option>
                            <option value="август">Август</option>
                            <option value="сентябрь">Сентябрь</option>
                            <option value="октябрь">Октябрь</option>
                            <option value="ноябрь">Ноябрь</option>
                            <option value="декабрь">Декабрь</option>
                        </select>
                        <button id="month-btn" class="tap-btn">Проверить ответ</button>
                    </div>
                    <div class="error-message" id="month-error">Неверный месяц, вспомни наши первые свидания</div>
                    <div class="success-message" id="month-success">Правильно! Как же я был счастлив в тот месяц!</div>
                    <div class="hint">Подсказка: это был теплый месяц, когда цветут деревья</div>
                </div>
                
                <div class="progress-container">
                    <div class="progress-bar" id="progress-bar-3"></div>
                </div>
            </div>
        </div>

        <!-- Страница 5: Задание 4 -->
        <div id="page5" class="page">
            <div class="page-content">
                <div class="task-counter">Задание 4 из 4</div>
                <h2>Последнее задание</h2>
                
                <p>Ты почти у цели, <span class="pet-name">зайчик</span>! Осталось последнее задание о цветах.</p>
                
                <div class="task-container">
                    <h3><span class="flower-icon">🌸</span> Математика для программиста <span class="flower-icon">🌸</span></h3>
                    
                    <div class="math-task">
                        <p>Программист купил 25 роз и решил подарить их самым важным женщинам в своей жизни:</p>
                        <ul>
                            <li>Маме он подарил <strong>нечётное</strong> количество роз</li>
                            <li>Сестре он тоже подарил <strong>нечётное</strong> количество роз</li>
                            <li>Тебе, <span class="pet-name">зайчик</span>, он хочет подарить <strong>больше всех</strong> роз</li>
                            <li>Всего у него 25 роз, и он хочет отдать их все</li>
                            <li>Каждой женщине он дарит разное количество роз</li>
                        </ul>
                        <p>Сколько роз получишь ты, <span class="pet-name">зайчик</span>?</p>
                    </div>
                    
                    <div class="input-container">
                        <input type="number" id="roses-input" placeholder="Введи число роз" inputmode="numeric" min="1" max="25">
                        <button id="roses-btn" class="tap-btn">Проверить ответ</button>
                    </div>
                    <div class="error-message" id="roses-error">Неверный ответ! Помни: тебе больше всех, всем нечётное число, всего 25 роз</div>
                    <div class="success-message" id="roses-success">Правильно! Конечно, тебе больше всех, ведь ты самая важная!</div>
                    <div class="hint">Подсказка: попробуй подобрать три разных нечётных числа, которые в сумме дают 25, где самое большое число - твоё</div>
                    
                    <div style="margin-top: 1.5rem; padding: 1rem; background-color: rgba(229, 213, 202, 0.3); border-radius: 10px;">
                        <h4>Варианты нечётных чисел до 25:</h4>
                        <p style="font-size: 0.9rem; margin-bottom: 0.5rem;">1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25</p>
                        <p style="font-size: 0.9rem;">Твоё число должно быть самым большим из трёх!</p>
                    </div>
                </div>
                
                <div class="progress-container">
                    <div class="progress-bar" id="progress-bar-4"></div>
                </div>
            </div>
        </div>

        <!-- Страница 6: Финальная -->
        <div id="page6" class="page">
            <div class="page-content">
                <div class="decoration">
                    <span>❦</span><span class="heart">♥</span><span>❦</span>
                </div>
                
                <h2>Поздравляю, зайчик!</h2>
                
                <p>Ты успешно прошла все испытания и доказала, что достойна быть <span class="highlight">жену программиста</span>!</p>
                
                <div class="message-box">
                    <div class="final-message">
                        <p>Мой самый любимый <span class="pet-name">зайчик</span>,</p>
                        <p>Ты — самый лучший код, который я когда-либо писал.</p>
                        <p>Ты — самое красивое исключение в моей программе.</p>
                        <p>Ты — бесконечный цикл счастья в моей жизни.</p>
                    </div>
                    
                    <div class="decoration">
                        <span>✽</span><span>❧</span><span>✽</span>
                    </div>
                    
                    <h3><span class="flower-icon">💐</span> Приглашаю тебя на свидание! <span class="flower-icon">💐</span></h3>
                    <p>В эту среду, в 13:00, в нашем любимом бильярде.</p>
                    <p>Я забронировал столик с очень удобными местами.</p>
                    
                    <div style="margin: 2rem 0;">
                        <button id="final-btn" class="tap-btn">С радостью принимаю приглашение!</button>
                    </div>
                </div>
                
                <p style="text-align: center; margin-top: 2rem;">Я так сильно тебя люблю, мой <span class="pet-name">зайчик</span>! <span class="heart">♥</span></p>
                
                <div class="decoration">
                    <span>❧</span><span class="heart">♥</span><span>❧</span>
                </div>
            </div>
        </div>
    </div>

    <script>
        // ГЛОБАЛЬНАЯ ПЕРЕМЕННАЯ ДЛЯ ОТЛАДКИ
        const DEBUG = true;
        
        // Состояние приложения
        const state = {
            petName: 'зайчик',
            currentPage: 1,
            totalPages: 6,
            answers: {
                apples: 1,
                codeName: 'зайчик',
                month: 'май', // ВАЖНО: замени на правильный месяц!
                roses: 15
            }
        };

        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', function() {
            if (DEBUG) console.log('🚀 DOM загружен, инициализация...');
            
            // Инициализация всех компонентов
            initializeApp();
            
            // Специальная настройка для iOS
            setupForIOS();
            
            if (DEBUG) console.log('✅ Квест готов к работе!');
        });

        // Основная функция инициализации
        function initializeApp() {
            // Настройка обработчиков для всех кнопок
            setupButtonHandlers();
            
            // Настройка обработчиков для полей ввода
            setupInputHandlers();
            
            // Инициализация прогресса
            initializeProgress();
        }

        // Настройка кнопок
        function setupButtonHandlers() {
            if (DEBUG) console.log('🔄 Настройка кнопок...');
            
            // Кнопка "Начать квест"
            const startBtn = document.getElementById('start-btn');
            if (startBtn) {
                addMobileClickHandler(startBtn, function() {
                    if (DEBUG) console.log('👉 Кнопка "Начать квест!" нажата');
                    navigateToPage(2);
                });
            }

            // Кнопка проверки яблок
            const appleBtn = document.getElementById('apple-btn');
            if (appleBtn) {
                addMobileClickHandler(appleBtn, checkAppleAnswer);
            }

            // Кнопка проверки кода
            const codeBtn = document.getElementById('code-btn');
            if (codeBtn) {
                addMobileClickHandler(codeBtn, checkCodeAnswer);
            }

            // Кнопка проверки месяца
            const monthBtn = document.getElementById('month-btn');
            if (monthBtn) {
                addMobileClickHandler(monthBtn, checkMonthAnswer);
            }

            // Кнопка проверки роз
            const rosesBtn = document.getElementById('roses-btn');
            if (rosesBtn) {
                addMobileClickHandler(rosesBtn, checkRosesAnswer);
            }

            // Финальная кнопка
            const finalBtn = document.getElementById('final-btn');
            if (finalBtn) {
                addMobileClickHandler(finalBtn, showFinalMessage);
            }
        }

        // Универсальный обработчик для мобильных устройств
        function addMobileClickHandler(element, callback) {
            if (!element || !callback) return;
            
            // Для touch устройств
            element.addEventListener('touchstart', function(e) {
                e.preventDefault();
                e.stopPropagation();
                
                // Визуальная обратная связь
                this.style.transform = 'scale(0.95)';
                this.style.opacity = '0.9';
                
                // Запуск колбэка
                setTimeout(() => callback(), 10);
            }, { passive: false });
            
            // Для мыши
            element.addEventListener('click', function(e) {
                e.preventDefault();
                callback();
            });
            
            // Восстановление состояния кнопки
            element.addEventListener('touchend', function() {
                this.style.transform = '';
                this.style.opacity = '';
            });
            
            element.addEventListener('touchcancel', function() {
                this.style.transform = '';
                this.style.opacity = '';
            });
        }

        // Настройка полей ввода
        function setupInputHandlers() {
            // Обработка Enter в полях ввода
            const appleInput = document.getElementById('apple-input');
            if (appleInput) {
                appleInput.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') checkAppleAnswer();
                });
            }

            const codeInput = document.getElementById('code-input');
            if (codeInput) {
                codeInput.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') checkCodeAnswer();
                });
            }

            const rosesInput = document.getElementById('roses-input');
            if (rosesInput) {
                rosesInput.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') checkRosesAnswer();
                });
            }
        }

        // Инициализация прогресса
        function initializeProgress() {
            updateProgress(1, 0);
        }

        // Проверка задания с яблоками
        function checkAppleAnswer() {
            if (DEBUG) console.log('🍎 Проверка задания с яблоками');
            
            const input = document.getElementById('apple-input');
            const error = document.getElementById('apple-error');
            const success = document.getElementById('apple-success');
            
            if (!input || !error || !success) return;
            
            const userAnswer = parseInt(input.value);
            
            if (userAnswer === state.answers.apples) {
                showSuccess(error, success, 2, 25);
            } else {
                showError(error, success);
                input.focus();
            }
        }

        // Проверка задания с кодом
        function checkCodeAnswer() {
            if (DEBUG) console.log('💻 Проверка задания с кодом');
            
            const input = document.getElementById('code-input');
            const error = document.getElementById('code-error');
            const success = document.getElementById('code-success');
            
            if (!input || !error || !success) return;
            
            const userAnswer = input.value.trim().toLowerCase();
            const acceptedAnswers = ['зайчик', 'зайка', 'зайчонок', 'зая'];
            
            if (acceptedAnswers.includes(userAnswer)) {
                showSuccess(error, success, 3, 50);
            } else {
                showError(error, success);
                input.focus();
            }
        }

        // Проверка задания с месяцем
        function checkMonthAnswer() {
            if (DEBUG) console.log('📅 Проверка задания с месяцем');
            
            const select = document.getElementById('month-select');
            const error = document.getElementById('month-error');
            const success = document.getElementById('month-success');
            
            if (!select || !error || !success) return;
            
            const userAnswer = select.value.toLowerCase();
            
            // Проверяем разные варианты написания
            const monthVariants = {
                'январь': ['январь', 'январе', 'january'],
                'февраль': ['февраль', 'феврале', 'february'],
                'март': ['март', 'марте', 'march'],
                'апрель': ['апрель', 'апреле', 'april'],
                'май': ['май', 'мае', 'may'],
                'июнь': ['июнь', 'июне', 'june'],
                'июль': ['июль', 'июле', 'july'],
                'август': ['август', 'августе', 'august'],
                'сентябрь': ['сентябрь', 'сентябре', 'september'],
                'октябрь': ['октябрь', 'октябре', 'october'],
                'ноябрь': ['ноябрь', 'ноябре', 'november'],
                'декабрь': ['декабрь', 'декабре', 'december']
            };
            
            let isCorrect = false;
            for (const month in monthVariants) {
                if (monthVariants[month].includes(userAnswer) && month === state.answers.month) {
                    isCorrect = true;
                    break;
                }
            }
            
            if (isCorrect) {
                showSuccess(error, success, 4, 75);
            } else {
                showError(error, success);
                select.focus();
            }
        }

        // Проверка задания с розами
        function checkRosesAnswer() {
            if (DEBUG) console.log('🌹 Проверка задания с розами');
            
            const input = document.getElementById('roses-input');
            const error = document.getElementById('roses-error');
            const success = document.getElementById('roses-success');
            
            if (!input || !error || !success) return;
            
            const userAnswer = parseInt(input.value);
            
            // Проверяем основные условия
            if (!userAnswer || userAnswer <= 0 || userAnswer > 25 || userAnswer % 2 === 0) {
                error.textContent = 'Число должно быть нечётным, положительным и не больше 25!';
                showError(error, success);
                input.focus();
                return;
            }
            
            // Ищем решение
            let foundSolution = false;
            let solution = '';
            
            for (let mom = 1; mom < userAnswer; mom += 2) {
                for (let sister = 1; sister < userAnswer; sister += 2) {
                    if (mom !== sister && mom !== userAnswer && sister !== userAnswer) {
                        if (mom + sister + userAnswer === 25) {
                            foundSolution = true;
                            solution = `Маме: ${mom} роз, сестре: ${sister} роз, тебе: ${userAnswer} роз`;
                            break;
                        }
                    }
                }
                if (foundSolution) break;
            }
            
            if (foundSolution) {
                success.textContent = `Правильно! ${solution}`;
                success.style.display = 'block';
                error.style.display = 'none';
                
                setTimeout(() => {
                    updateProgress(5, 100);
                    navigateToPage(6);
                }, 2000);
            } else {
                error.textContent = 'Неверно! Нельзя подобрать другие нечётные числа. Попробуй другое число!';
                showError(error, success);
                input.focus();
            }
        }

        // Показать успех
        function showSuccess(errorElement, successElement, nextPage, progressPercent) {
            successElement.style.display = 'block';
            errorElement.style.display = 'none';
            
            setTimeout(() => {
                successElement.style.display = 'none';
                updateProgress(nextPage, progressPercent);
                navigateToPage(nextPage);
            }, 1000);
        }

        // Показать ошибку
        function showError(errorElement, successElement) {
            errorElement.style.display = 'block';
            successElement.style.display = 'none';
        }

        // Навигация между страницами
        function navigateToPage(pageNumber) {
            if (DEBUG) console.log(`🔄 Переход на страницу ${pageNumber}`);
            
            if (pageNumber < 1 || pageNumber > state.totalPages) return;
            
            const currentPage = document.getElementById(`page${state.currentPage}`);
            const newPage = document.getElementById(`page${pageNumber}`);
            
            if (!currentPage || !newPage) return;
            
            // Убираем класс active у текущей страницы
            currentPage.classList.remove('active');
            
            // Добавляем класс previous для анимации
            currentPage.classList.add('previous');
            
            // Показываем новую страницу
            setTimeout(() => {
                newPage.classList.remove('previous');
                newPage.classList.add('active');
                state.currentPage = pageNumber;
                
                // Прокручиваем наверх
                newPage.scrollTop = 0;
                
                // Фокусируемся на первом поле ввода
                setTimeout(() => {
                    const firstInput = newPage.querySelector('input, select');
                    if (firstInput) {
                        firstInput.focus();
                        if (DEBUG) console.log('🎯 Фокус установлен на поле ввода');
                    }
                }, 100);
            }, 50);
        }

        // Обновление прогресс-бара
        function updateProgress(pageNumber, percentage) {
            const progressBar = document.getElementById(`progress-bar-${pageNumber - 1}`);
            if (progressBar) {
                progressBar.style.width = `${percentage}%`;
                if (DEBUG) console.log(`📊 Прогресс: ${percentage}%`);
            }
        }

        // Финальное сообщение
        function showFinalMessage() {
            if (DEBUG) console.log('🎉 Показать финальное сообщение');
            
            const finalBtn = document.getElementById('final-btn');
            if (finalBtn) {
                finalBtn.textContent = "Я тоже тебя люблю, зайчик! ❤️";
                finalBtn.disabled = true;
                
                // Создаем конфетти
                createConfetti();
                
                // Добавляем дополнительное сообщение
                setTimeout(() => {
                    const messageBox = finalBtn.closest('.message-box');
                    if (messageBox) {
                        const extraMessage = document.createElement('p');
                        extraMessage.style.cssText = 'margin-top: 1rem; font-size: 1.2rem; color: var(--accent); font-weight: bold;';
                        extraMessage.textContent = 'Жду нашу встречу, мой зайчик! Буду считать часы до среды!';
                        messageBox.appendChild(extraMessage);
                    }
                }, 1000);
            }
        }

        // Создание конфетти
        function createConfetti() {
            if (DEBUG) console.log('🎊 Создание конфетти');
            
            const colors = ['#EDEDE9', '#D6CCC2', '#F5EBE0', '#E3D5CA', '#D5BDAF', '#9D6B53'];
            const count = 80;
            
            for (let i = 0; i < count; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                confetti.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.top = '-10px';
                
                document.body.appendChild(confetti);
                
                // Анимация
                const animation = confetti.animate([
                    { 
                        transform: 'translateY(0) rotate(0deg)',
                        opacity: 1 
                    },
                    { 
                        transform: `translateY(${window.innerHeight + 20}px) rotate(${Math.random() * 360}deg)`,
                        opacity: 0 
                    }
                ], {
                    duration: 2000 + Math.random() * 2000,
                    easing: 'cubic-bezier(0.215, 0.610, 0.355, 1)'
                });
                
                animation.onfinish = () => confetti.remove();
            }
        }

        // Специальная настройка для iOS
        function setupForIOS() {
            const isIOS = /iPad|iPhone|iPod/.test(navigator.userAgent) && !window.MSStream;
            
            if (isIOS) {
                if (DEBUG) console.log('📱 Обнаружен iOS, применяем специальные настройки');
                
                // Предотвращаем масштабирование при фокусе
                document.addEventListener('touchstart', function() {}, { passive: true });
                
                // Исправляем поведение полей ввода
                const inputs = document.querySelectorAll('input, select, textarea');
                inputs.forEach(input => {
                    input.addEventListener('touchstart', function(e) {
                        e.stopPropagation();
                    });
                });
                
                // Улучшаем отзывчивость кнопок
                const buttons = document.querySelectorAll('button');
                buttons.forEach(button => {
                    button.style.cursor = 'pointer';
                });
            }
        }

        // Обработка ошибок
        window.addEventListener('error', function(e) {
            if (DEBUG) {
                console.error('❌ Ошибка:', e.message);
                console.error('Файл:', e.filename);
                console.error('Строка:', e.lineno);
            }
        });

        // Предотвращаем нежелательное поведение на iOS
        document.addEventListener('touchmove', function(e) {
            if (e.target.tagName === 'INPUT' || e.target.tagName === 'SELECT' || e.target.tagName === 'TEXTAREA') {
                return;
            }
            
            // Предотвращаем скролл страницы при анимациях
            if (e.target.closest('.page-content')) {
                e.stopPropagation();
            }
        }, { passive: false });

        // Устанавливаем правильный размер viewport при изменении ориентации
        window.addEventListener('orientationchange', function() {
            setTimeout(() => {
                document.body.style.height = window.innerHeight + 'px';
            }, 100);
        });

        // Предотвращаем контекстное меню на кнопках
        document.addEventListener('contextmenu', function(e) {
            if (e.target.tagName === 'BUTTON') {
                e.preventDefault();
            }
        });

        // ВАЖНО: Не забудьте установить правильный месяц!
        // state.answers.month = 'ваш_месяц'; // Например: 'май', 'июнь', 'июль' и т.д.
    </script>
</body>
</html>
