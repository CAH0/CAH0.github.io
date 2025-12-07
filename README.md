<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Специальный квест для моего зайчика</title>
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400;700&family=Roboto:wght@300;400;500&display=swap">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --cream: #EDEDE9;
            --light-tan: #D6CCC2;
            --light-beige: #F5EBE0;
            --tan: #E3D5CA;
            --dark-tan: #D5BDAF;
            --text-dark: #5D534A;
            --accent: #9D6B53;
        }

        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--cream);
            color: var(--text-dark);
            line-height: 1.6;
            min-height: 100vh;
            background-image: linear-gradient(to bottom, var(--cream) 0%, var(--light-beige) 100%);
        }

        .page {
            display: none;
            min-height: 100vh;
            padding: 2rem;
            max-width: 800px;
            margin: 0 auto;
            opacity: 0;
            transition: opacity 0.8s ease;
        }

        .page.active {
            display: block;
            opacity: 1;
        }

        .page-content {
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 20px;
            padding: 3rem;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.5);
            position: relative;
            overflow: hidden;
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

        h1, h2 {
            font-family: 'Dancing Script', cursive;
            color: var(--accent);
            margin-bottom: 1.5rem;
            text-align: center;
        }

        h1 {
            font-size: 3.5rem;
            margin-bottom: 2rem;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
        }

        h2 {
            font-size: 2.8rem;
            margin-bottom: 1.5rem;
        }

        h3 {
            font-size: 1.5rem;
            color: var(--accent);
            margin-bottom: 1rem;
        }

        p {
            margin-bottom: 1.5rem;
            font-size: 1.2rem;
        }

        .highlight {
            background-color: rgba(213, 189, 175, 0.3);
            padding: 0.3rem 0.6rem;
            border-radius: 5px;
            font-weight: 500;
        }

        .pet-name {
            color: var(--accent);
            font-weight: bold;
            font-style: italic;
            font-size: 1.3rem;
        }

        .task-container {
            background-color: var(--light-beige);
            padding: 1.5rem;
            border-radius: 15px;
            margin: 2rem 0;
            border: 1px dashed var(--tan);
        }

        .code-block {
            background-color: var(--cream);
            padding: 1.5rem;
            border-radius: 10px;
            font-family: monospace;
            font-size: 1.1rem;
            margin: 1.5rem 0;
            border-left: 5px solid var(--dark-tan);
            overflow-x: auto;
        }

        .input-container {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
            flex-wrap: wrap;
        }

        input[type="text"], input[type="number"], select {
            flex: 1;
            padding: 0.8rem 1.2rem;
            border: 2px solid var(--tan);
            border-radius: 10px;
            font-size: 1.1rem;
            min-width: 200px;
            background-color: white;
        }

        input[type="text"]:focus, input[type="number"]:focus, select:focus {
            outline: none;
            border-color: var(--accent);
        }

        button {
            background-color: var(--accent);
            color: white;
            border: none;
            padding: 0.8rem 2rem;
            border-radius: 10px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 500;
        }

        button:hover {
            background-color: var(--dark-tan);
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        button:disabled {
            background-color: var(--light-tan);
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

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
            margin: 2rem 0;
        }

        .decoration span {
            font-size: 2.5rem;
            margin: 0 0.5rem;
            color: var(--dark-tan);
        }

        .message-box {
            background-color: var(--light-beige);
            padding: 2.5rem;
            border-radius: 15px;
            margin: 2rem 0;
            text-align: center;
            border: 2px solid var(--tan);
        }

        .final-message {
            font-family: 'Dancing Script', cursive;
            font-size: 2.2rem;
            line-height: 1.4;
            color: var(--accent);
            margin-bottom: 2rem;
        }

        .instructions {
            background-color: rgba(229, 213, 202, 0.3);
            padding: 1.5rem;
            border-radius: 10px;
            margin: 1.5rem 0;
            font-size: 1.1rem;
            border-left: 4px solid var(--dark-tan);
        }

        .task-counter {
            display: inline-block;
            background-color: var(--accent);
            color: white;
            padding: 0.3rem 1rem;
            border-radius: 20px;
            font-size: 0.9rem;
            margin-bottom: 1rem;
        }

        .progress-container {
            margin: 2rem 0;
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
        }

        .success-message {
            color: var(--accent);
            margin-top: 0.5rem;
            font-weight: 500;
            display: none;
        }

        .hint {
            color: var(--accent);
            font-style: italic;
            margin-top: 0.5rem;
            font-size: 0.9rem;
        }

        .welcome-message {
            text-align: center;
            padding: 1.5rem;
            background: linear-gradient(135deg, rgba(245, 235, 224, 0.7), rgba(227, 213, 202, 0.7));
            border-radius: 15px;
            margin: 2rem 0;
        }

        .flower-icon {
            color: #e63946;
            font-size: 1.2rem;
            margin: 0 0.2rem;
        }

        .math-task {
            background-color: rgba(229, 213, 202, 0.5);
            padding: 1.5rem;
            border-radius: 10px;
            margin: 1rem 0;
        }

        .math-task ul {
            margin-left: 1.5rem;
            margin-bottom: 1rem;
        }

        .math-task li {
            margin-bottom: 0.5rem;
        }

        @media (max-width: 768px) {
            .page {
                padding: 1rem;
            }
            
            .page-content {
                padding: 2rem 1.5rem;
            }
            
            h1 {
                font-size: 2.8rem;
            }
            
            h2 {
                font-size: 2.2rem;
            }
            
            .input-container {
                flex-direction: column;
            }
            
            input[type="text"], input[type="number"], select {
                min-width: 100%;
            }
        }
    </style>
</head>
<body>
    <!-- Приветственная страница -->
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
                    <button id="start-btn">Начать квест!</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Страница с заданием 1 -->
    <div id="page2" class="page">
        <div class="page-content">
            <div class="task-counter">Задание 1 из 4</div>
            <h2>Первое задание</h2>
            
            <p>Привет, <span class="pet-name">зайчик</span>! Как <span class="highlight">жена программиста</span>, ты должна знать кое-что о логике.</p>
            
            <div class="task-container">
                <h3>Логическая задачка</h3>
                <p>У программиста есть два яблока. Одно он отдает тебе, другое - оставляет себе. Сколько яблок осталось у программиста?</p>
                
                <div class="input-container">
                    <input type="number" id="apple-input" placeholder="Введи число">
                    <button id="apple-btn">Проверить ответ</button>
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

    <!-- Страница с заданием 2 -->
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
                    <input type="text" id="code-input" placeholder="Введи ласковое прозвище">
                    <button id="code-btn">Проверить ответ</button>
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

    <!-- Страница с заданием 3 -->
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
                    <button id="month-btn">Проверить ответ</button>
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

    <!-- Страница с заданием 4 -->
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
                    <input type="number" id="roses-input" placeholder="Введи число роз">
                    <button id="roses-btn">Проверить ответ</button>
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

    <!-- Финальная страница с приглашением -->
    <div id="page6" class="page">
        <div class="page-content">
            <div class="decoration">
                <span>❦</span><span class="heart">♥</span><span>❦</span>
            </div>
            
            <h2>Поздравляю, зайчик!</h2>
            
            <p>Ты успешно прошла все испытания и доказала, что достойна быть <span class="highlight">женой программиста</span>!</p>
            
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
                    <button id="final-btn">С радостью принимаю приглашение!</button>
                </div>
            </div>
            
            <p style="text-align: center; margin-top: 2rem;">Я так сильно тебя люблю, мой <span class="pet-name">зайчик</span>! <span class="heart">♥</span></p>
            
            <div class="decoration">
                <span>❧</span><span class="heart">♥</span><span>❧</span>
            </div>
        </div>
    </div>

    <script>
        // Состояние приложения
        const state = {
            petName: 'зайчик', // Ласковое имя, которое вы используете
            currentPage: 1,
            totalPages: 6,
            answers: {
                apples: 1,
                codeName: 'зайчик',
                month: 'май', // Замените на правильный месяц
                roses: 15 // Правильный ответ: 15 роз (маме 3, сестре 7, зайчику 15)
            }
        };

        // Элементы страниц
        const pages = document.querySelectorAll('.page');
        const progressBars = document.querySelectorAll('.progress-bar');
        
        // Элементы первой страницы
        const startBtn = document.getElementById('start-btn');
        
        // Элементы второй страницы
        const appleInput = document.getElementById('apple-input');
        const appleBtn = document.getElementById('apple-btn');
        const appleError = document.getElementById('apple-error');
        const appleSuccess = document.getElementById('apple-success');
        
        // Элементы третьей страницы
        const codeInput = document.getElementById('code-input');
        const codeBtn = document.getElementById('code-btn');
        const codeError = document.getElementById('code-error');
        const codeSuccess = document.getElementById('code-success');
        
        // Элементы четвертой страницы
        const monthSelect = document.getElementById('month-select');
        const monthBtn = document.getElementById('month-btn');
        const monthError = document.getElementById('month-error');
        const monthSuccess = document.getElementById('month-success');
        
        // Элементы пятой страницы
        const rosesInput = document.getElementById('roses-input');
        const rosesBtn = document.getElementById('roses-btn');
        const rosesError = document.getElementById('roses-error');
        const rosesSuccess = document.getElementById('roses-success');
        
        // Элементы финальной страницы
        const finalBtn = document.getElementById('final-btn');

        // Инициализация приложения
        function init() {
            // Настройка обработчиков событий для первой страницы
            startBtn.addEventListener('click', () => {
                navigateToPage(2);
            });
            
            // Настройка обработчиков для второй страницы
            appleBtn.addEventListener('click', checkAppleAnswer);
            appleInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') checkAppleAnswer();
            });
            
            // Настройка обработчиков для третьей страницы
            codeBtn.addEventListener('click', checkCodeAnswer);
            codeInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') checkCodeAnswer();
            });
            
            // Настройка обработчиков для четвертой страницы
            monthBtn.addEventListener('click', checkMonthAnswer);
            
            // Настройка обработчиков для пятой страницы
            rosesBtn.addEventListener('click', checkRosesAnswer);
            rosesInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') checkRosesAnswer();
            });
            
            // Настройка обработчика для финальной страницы
            finalBtn.addEventListener('click', showFinalMessage);
            
            // Подставляем ласковое имя везде, где это необходимо
            updatePetNameInContent();
        }
        
        // Обновляем ласковое имя во всем контенте
        function updatePetNameInContent() {
            console.log(`Ласковое имя установлено: ${state.petName}`);
        }
        
        // Проверка ответа на задание с яблоками
        function checkAppleAnswer() {
            const userAnswer = parseInt(appleInput.value);
            
            if (userAnswer === state.answers.apples) {
                appleError.style.display = 'none';
                appleSuccess.style.display = 'block';
                setTimeout(() => {
                    appleSuccess.style.display = 'none';
                    updateProgress(2, 25);
                    navigateToPage(3);
                }, 1000);
            } else {
                appleError.style.display = 'block';
                appleSuccess.style.display = 'none';
                appleInput.focus();
            }
        }
        
        // Проверка ответа на задание с кодом
        function checkCodeAnswer() {
            const userAnswer = codeInput.value.trim().toLowerCase();
            const correctAnswer = state.petName.toLowerCase();
            
            // Принимаем разные варианты написания
            if (userAnswer === correctAnswer || userAnswer === 'зайка' || userAnswer === 'зайчонок' || userAnswer === 'зая') {
                codeError.style.display = 'none';
                codeSuccess.style.display = 'block';
                setTimeout(() => {
                    codeSuccess.style.display = 'none';
                    updateProgress(3, 50);
                    navigateToPage(4);
                }, 1000);
            } else {
                codeError.style.display = 'block';
                codeSuccess.style.display = 'none';
                codeInput.focus();
            }
        }
        
        // Проверка ответа на задание с месяцем
        function checkMonthAnswer() {
            const userAnswer = monthSelect.value.toLowerCase();
            const correctAnswer = state.answers.month.toLowerCase();
            
            // Принимаем разные варианты написания месяцев
            const monthVariants = {
                'май': ['май', 'мае', 'may'],
                'апрель': ['апрель', 'апреле', 'april'],
                'июнь': ['июнь', 'июне', 'june']
            };
            
            let isCorrect = false;
            
            // Проверяем по каждому возможному месяцу
            for (const month in monthVariants) {
                if (monthVariants[month].includes(userAnswer) && month === correctAnswer) {
                    isCorrect = true;
                    break;
                }
            }
            
            if (isCorrect) {
                monthError.style.display = 'none';
                monthSuccess.style.display = 'block';
                setTimeout(() => {
                    monthSuccess.style.display = 'none';
                    updateProgress(4, 75);
                    navigateToPage(5);
                }, 1000);
            } else {
                monthError.style.display = 'block';
                monthSuccess.style.display = 'none';
                monthSelect.focus();
            }
        }
        
        // Проверка ответа на задание с розами
        function checkRosesAnswer() {
            const userAnswer = parseInt(rosesInput.value);
            
            // Проверяем, что число нечётное, больше 0 и меньше или равно 25
            if (userAnswer > 0 && userAnswer <= 25 && userAnswer % 2 !== 0) {
                // Проверяем, можно ли подобрать два других нечётных числа, которые в сумме с этим дадут 25
                // И при этом все три числа разные и userAnswer - наибольшее
                let foundSolution = false;
                
                // Перебираем возможные варианты для мамы и сестры
                for (let mom = 1; mom < userAnswer; mom += 2) {
                    for (let sister = 1; sister < userAnswer; sister += 2) {
                        // Проверяем, что все числа разные
                        if (mom !== sister && mom !== userAnswer && sister !== userAnswer) {
                            // Проверяем сумму
                            if (mom + sister + userAnswer === 25) {
                                foundSolution = true;
                                break;
                            }
                        }
                    }
                    if (foundSolution) break;
                }
                
                if (foundSolution) {
                    rosesError.style.display = 'none';
                    rosesSuccess.style.display = 'block';
                    
                    // Показываем решение
                    setTimeout(() => {
                        // Находим конкретное решение для показа
                        let solution = '';
                        for (let mom = 1; mom < userAnswer; mom += 2) {
                            for (let sister = 1; sister < userAnswer; sister += 2) {
                                if (mom !== sister && mom !== userAnswer && sister !== userAnswer && 
                                    mom + sister + userAnswer === 25) {
                                    solution = `Маме: ${mom} роз, сестре: ${sister} роз, тебе: ${userAnswer} роз`;
                                    break;
                                }
                            }
                            if (solution) break;
                        }
                        
                        // Добавляем информацию о решении
                        const solutionInfo = document.createElement('div');
                        solutionInfo.className = 'hint';
                        solutionInfo.style.color = 'var(--accent)';
                        solutionInfo.style.fontWeight = 'bold';
                        solutionInfo.style.marginTop = '1rem';
                        solutionInfo.innerHTML = `Правильно! ${solution}`;
                        
                        // Убираем старое сообщение об успехе и добавляем новое
                        rosesSuccess.style.display = 'none';
                        rosesBtn.parentNode.appendChild(solutionInfo);
                        
                        // Через 2 секунды переходим дальше
                        setTimeout(() => {
                            updateProgress(5, 100);
                            navigateToPage(6);
                        }, 2000);
                    }, 1000);
                } else {
                    rosesError.style.display = 'block';
                    rosesSuccess.style.display = 'none';
                    rosesError.textContent = 'Неверно! Нельзя подобрать другие нечётные числа, чтобы в сумме было 25. Попробуй другое число!';
                    rosesInput.focus();
                }
            } else {
                rosesError.style.display = 'block';
                rosesSuccess.style.display = 'none';
                rosesError.textContent = 'Число должно быть нечётным, положительным и не больше 25!';
                rosesInput.focus();
            }
        }
        
        // Финальное сообщение
        function showFinalMessage() {
            finalBtn.textContent = "Я тоже тебя люблю, зайчик! ❤️";
            finalBtn.disabled = true;
            
            // Создаем эффект конфетти
            createConfetti();
            
            // Добавляем дополнительное сообщение
            setTimeout(() => {
                const newMessage = document.createElement('p');
                newMessage.innerHTML = `<p style="margin-top: 1rem; font-size: 1.2rem; color: var(--accent);">Жду нашу встречу, мой <span class="pet-name">зайчик</span>! Буду считать часы до среды!</p>`;
                finalBtn.parentNode.appendChild(newMessage);
                
      
            }, 1000);
        }
        
        // Навигация между страницами
        function navigateToPage(pageNumber) {
            // Скрываем текущую страницу
            document.getElementById(`page${state.currentPage}`).classList.remove('active');
            
            // Показываем новую страницу
            document.getElementById(`page${pageNumber}`).classList.add('active');
            
            // Обновляем текущую страницу
            state.currentPage = pageNumber;
            
            // Прокручиваем наверх
            window.scrollTo(0, 0);
            
            // Фокусируемся на первом поле ввода на новой странице
            setTimeout(() => {
                const inputs = document.querySelectorAll(`#page${pageNumber} input[type="text"], #page${pageNumber} input[type="number"], #page${pageNumber} select`);
                if (inputs.length > 0) {
                    inputs[0].focus();
                }
            }, 300);
        }
        
        // Обновление прогресс-бара
        function updateProgress(pageNumber, percentage) {
            const progressBar = document.getElementById(`progress-bar-${pageNumber-1}`);
            if (progressBar) {
                progressBar.style.width = `${percentage}%`;
            }
        }
        
        // Создание эффекта конфетти
        function createConfetti() {
            const colors = ['#EDEDE9', '#D6CCC2', '#F5EBE0', '#E3D5CA', '#D5BDAF', '#9D6B53'];
            const confettiCount = 100;
            
            for (let i = 0; i < confettiCount; i++) {
                const confetti = document.createElement('div');
                confetti.style.position = 'fixed';
                confetti.style.width = '10px';
                confetti.style.height = '10px';
                confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                confetti.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
                confetti.style.top = '-20px';
                confetti.style.left = `${Math.random() * 100}vw`;
                confetti.style.opacity = '0.8';
                confetti.style.zIndex = '9999';
                confetti.style.pointerEvents = 'none';
                
                document.body.appendChild(confetti);
                
                // Анимация падения
                const animation = confetti.animate([
                    { transform: `translate(0, 0) rotate(0deg)`, opacity: 0.8 },
                    { transform: `translate(${Math.random() * 100 - 50}px, ${window.innerHeight + 20}px) rotate(${Math.random() * 360}deg)`, opacity: 0 }
                ], {
                    duration: 2000 + Math.random() * 3000,
                    easing: 'cubic-bezier(0.215, 0.61, 0.355, 1)'
                });
                
                // Удаление конфетти после анимации
                animation.onfinish = () => {
                    confetti.remove();
                };
            }
        }
        
        // Важное замечание для вас:
        // Не забудьте изменить state.answers.month на правильный месяц вашего знакомства!
        // Строка 370: замените 'май' на правильный месяц
        
        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
