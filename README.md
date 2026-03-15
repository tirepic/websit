<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Irregular Verbs Game</title>
    <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FF9F43;
            --secondary-color: #54a0ff;
            --success-color: #1dd1a1;
            --error-color: #ff6b6b;
            --hint-color: #a55eea;
            --text-color: #2f3542;
            --bg-overlay: rgba(255, 255, 255, 0.85);
        }

        body {
            font-family: 'Comic Neue', cursive, sans-serif;
            margin: 0;
            padding: 0;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-image: url('https://image.qwenlm.ai/public_source/6b565c00-7271-4c59-a638-684fdf61e9d9/1d66ec308-2343-4237-8017-8e4a94b927e7.png');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            color: var(--text-color);
        }

        .game-container {
            background-color: var(--bg-overlay);
            backdrop-filter: blur(5px);
            border-radius: 25px;
            padding: 2rem;
            width: 90%;
            max-width: 800px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        header {
            margin-bottom: 1.5rem;
        }

        h1 {
            color: var(--secondary-color);
            font-size: 2.5rem;
            margin: 0;
            text-shadow: 2px 2px 0px #fff;
        }

        .progress-bar {
            background-color: #dfe4ea;
            border-radius: 15px;
            height: 20px;
            width: 100%;
            margin-top: 10px;
            overflow: hidden;
        }

        .progress-fill {
            background-color: var(--success-color);
            height: 100%;
            width: 0%;
            transition: width 0.5s ease;
        }

        .verse-card {
            background: white;
            border-radius: 15px;
            padding: 2rem;
            margin-bottom: 2rem;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
            border: 3px solid #f1f2f6;
        }

        .verse-text {
            font-size: 1.4rem;
            line-height: 1.6;
            margin-bottom: 1.5rem;
            color: #57606f;
        }

        .blank-space {
            display: inline-block;
            border-bottom: 3px dashed var(--primary-color);
            color: var(--primary-color);
            font-weight: bold;
            min-width: 80px;
            text-align: center;
            margin: 0 5px;
        }

        .inputs-area {
            display: flex;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 1.5rem;
        }

        .input-wrapper {
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .input-label {
            font-size: 0.8rem;
            color: #747d8c;
            margin-bottom: 5px;
        }

        input[type="text"] {
            font-family: 'Comic Neue', cursive, sans-serif;
            font-size: 1.2rem;
            padding: 10px;
            border: 2px solid #ced6e0;
            border-radius: 10px;
            width: 120px;
            text-align: center;
            transition: all 0.3s;
            outline: none;
        }

        input[type="text"]:focus {
            border-color: var(--secondary-color);
            box-shadow: 0 0 8px rgba(84, 160, 255, 0.3);
        }

        input.correct {
            border-color: var(--success-color);
            background-color: #e3f9e5;
            color: #10ac84;
        }

        input.wrong {
            border-color: var(--error-color);
            background-color: #ffeaa7;
            animation: shake 0.5s;
        }

        input.hint-used {
            border-color: var(--hint-color);
            background-color: #f3e5f5;
            color: var(--hint-color);
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
        }

        button {
            font-family: 'Comic Neue', cursive, sans-serif;
            font-size: 1.2rem;
            font-weight: bold;
            padding: 12px 30px;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 4px 0 rgba(0,0,0,0.1);
        }

        button:active {
            transform: translateY(4px);
            box-shadow: none;
        }

        button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .btn-check {
            background-color: var(--primary-color);
            color: white;
        }

        .btn-next {
            background-color: var(--secondary-color);
            color: white;
            display: none;
        }

        .btn-hint {
            background-color: var(--hint-color);
            color: white;
        }

        .btn-restart {
            background-color: var(--success-color);
            color: white;
            display: none;
        }

        .feedback-message {
            height: 30px;
            margin-top: 10px;
            font-weight: bold;
            font-size: 1.2rem;
        }

        .success-msg { color: var(--success-color); }
        .error-msg { color: var(--error-color); }
        .hint-msg { color: var(--hint-color); }

        .translation-hint {
            font-size: 1rem;
            color: #a4b0be;
            margin-top: 5px;
            font-style: italic;
        }

        @keyframes shake {
            0% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            50% { transform: translateX(5px); }
            75% { transform: translateX(-5px); }
            100% { transform: translateX(0); }
        }

        /* Confetti Canvas */
        #confetti-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 100;
        }

        @media (max-width: 600px) {
            h1 { font-size: 1.8rem; }
            .verse-text { font-size: 1.1rem; }
            input[type="text"] { width: 90px; font-size: 1rem; }
            .inputs-area { gap: 5px; }
            .controls { gap: 10px; }
            button { padding: 10px 20px; font-size: 1rem; }
        }
    </style>
</head>
<body>

<div class="game-container">
    <canvas id="confetti-canvas"></canvas>
    <header>
        <h1>Irregular Verbs</h1>
        <div class="progress-bar">
            <div class="progress-fill" id="progress-fill"></div>
        </div>
        <div style="margin-top: 5px; font-size: 0.9rem; color: #747d8c;">
            Уровень <span id="current-level">1</span> из <span id="total-levels">80</span>
        </div>
    </header>

    <div class="verse-card">
        <div class="verse-text" id="verse-display">
            <!-- Verse content goes here -->
        </div>
        <div class="translation-hint" id="translation-display"></div>
    </div>

    <div class="inputs-area" id="inputs-container">
        <!-- Inputs generated by JS -->
    </div>

    <div class="feedback-message" id="feedback"></div>

    <div class="controls">
        <button class="btn-hint" id="hint-btn" onclick="showHint()">💡 Подсказка (1 форма)</button>
        <button class="btn-check" id="check-btn" onclick="checkAnswer()">Проверить</button>
        <button class="btn-next" id="next-btn" onclick="nextLevel()">Дальше ➜</button>
        <button class="btn-restart" id="restart-btn" onclick="restartGame()">🔄 Заново</button>
    </div>
</div>

<script>
    // Data parsed from the user's poem
    const versesData = [
        { id: 1, text: "Море спорит с легким бризом, шторм ___, ___, ___", trans: "(подниматься, возникнуть)", ans: ["arise", "arose", "arisen"] },
        { id: 2, text: "Знайте все - глагол to be в детстве был ___, ___, ___", trans: "(быть)", ans: ["was", "were", "been"] },
        { id: 3, text: "Он неправильным рожден. Не забудьте: ___, ___, ___", trans: "(родить, нести)", ans: ["bear", "bore", "born"] },
        { id: 4, text: "Если к 'be' прижмется 'come' - слово будет новым нам, как ___, ___, ___", trans: "(сделать, стать)", ans: ["become", "became", "become"] },
        { id: 5, text: "Если 'be' имеет 'gun' - хулиганить хулиган вдруг ___, ___, ___", trans: "(начинать/ся)", ans: ["begin", "began", "begun"] },
        { id: 6, text: "Пользы нет от сигарет - они тело ___, ___, ___", trans: "(со)гнуть)", ans: ["bend", "bent", "bent"] },
        { id: 7, text: "Сожалеть не перестанут те, кто с ними ___, ___, ___", trans: "(связать)", ans: ["bind", "bound", "bound"] },
        { id: 8, text: "Если улей раздразнить - пчелы больно ___, ___, ___", trans: "(кусать (ся))", ans: ["bite", "bit", "bit"] },
        { id: 9, text: "Надо срочно в лазарет если рана ___, ___, ___", trans: "(кровоточить)", ans: ["bleed", "bled", "bled"] },
        { id: 10, text: "Я секрета не открою, то, что ветер ___, ___, ___", trans: "(дуть)", ans: ["blow", "blew", "blown"] },
        { id: 11, text: "Для всего есть в жизни сроки: все когда-то ___, ___, ___", trans: "(с)ломать)", ans: ["break", "broke", "broken"] },
        { id: 12, text: "Я Вам дам один совет: деток надо ___, ___, ___", trans: "(выращивать, воспитывать)", ans: ["breed", "bred", "bred"] },
        { id: 13, text: "Стюардесса чай несет - по-английски ___, ___, ___", trans: "(приносить)", ans: ["bring", "brought", "brought"] },
        { id: 14, text: "Тем, кто строил, не забыть про глаголы: ___, ___, ___", trans: "(строить)", ans: ["build", "built", "built"] },
        { id: 15, text: "Всем тепло огонь дает потому, что ___, ___, ___", trans: "(гореть)", ans: ["burn", "burnt", "burnt"] },
        { id: 16, text: "Колбасу на бутерброд - всё, что нужно ___, ___, ___", trans: "(покупать)", ans: ["buy", "bought", "bought"] },
        { id: 17, text: "Шел в ночи чернющий кот, рыбу где-то ___, ___, ___", trans: "(получать, становиться)", ans: ["get", "got", "got"] },
        { id: 18, text: "Жизнь тому добра не даст, кто в беде вас ___, ___, ___", trans: "(кидать, лить металл)", ans: ["cast", "cast", "cast"] },
        { id: 19, text: "Спит ленивый толстый кот, он мышей не ___, ___, ___", trans: "(ловить, схватывать)", ans: ["catch", "caught", "caught"] },
        { id: 20, text: "В жизни часто выбор сложен как нам все же ___, ___, ___?", trans: "(выбирать)", ans: ["choose", "chose", "chosen"] },
        { id: 21, text: "Будут все не рады Вам если часто ___, ___, ___", trans: "(приходить)", ans: ["come", "came", "come"] },
        { id: 22, text: "Для богатых - не вопрос: Сколько стоит ___, ___, ___?", trans: "(стоить)", ans: ["cost", "cost", "cost"] },
        { id: 23, text: "На Востоке есть обряд... Слово резать - ___, ___, ___", trans: "(резать)", ans: ["cut", "cut", "cut"] },
        { id: 24, text: "Ям не рой другим, чудак, и не стоит ___, ___, ___", trans: "(копать)", ans: ["dig", "dug", "dug"] },
        { id: 25, text: "Взял сынок большой картон и картину ___, ___, ___", trans: "(рисовать, тащить)", ans: ["draw", "drew", "drawn"] },
        { id: 26, text: "Будет талия у дам, коль зарядку ___, ___, ___", trans: "(делать)", ans: ["do", "did", "done"] },
        { id: 27, text: "Говорит всегда эстет: как прекрасно ___, ___, ___!", trans: "(мечтать, видеть во сне)", ans: ["dream", "dreamt", "dreamt"] },
        { id: 28, text: "Тот, кто выпить не дурак, любит слово ___, ___, ___", trans: "(пить)", ans: ["drink", "drank", "drunk"] },
        { id: 29, text: "Если Вы авто водили, вам знакомо ___, ___, ___", trans: "(водить, гнать)", ans: ["drive", "drove", "driven"] },
        { id: 30, text: "Если муж пришел сердитый - дайте кушать - ___, ___, ___", trans: "(кушать)", ans: ["eat", "ate", "eaten"] },
        { id: 31, text: "Каждый будет недоволен, если больно ___, ___, ___", trans: "(падать)", ans: ["fall", "fell", "fallen"] },
        { id: 32, text: "Наша кошка Пуси-Кэт Деток любит ___, ___, ___", trans: "(кормить)", ans: ["feed", "fed", "fed"] },
        { id: 33, text: "Сердцем чувствует поэт... Это слово ___, ___, ___", trans: "(чувствовать)", ans: ["feel", "felt", "felt"] },
        { id: 34, text: "На Земле любой народ за свободу ___, ___, ___", trans: "(сражаться)", ans: ["fight", "fought", "fought"] },
        { id: 35, text: "Отыскал боксер нокаут. Это слово ___, ___, ___", trans: "(находить)", ans: ["find", "found", "found"] },
        { id: 36, text: "Не дурите, мой совет, чтоб потом не ___, ___, ___", trans: "(бежать, спасаться)", ans: ["flee", "fled", "fled"] },
        { id: 37, text: "Во все страны Роллингстоун самолетом ___, ___, ___", trans: "(лететь)", ans: ["fly", "flew", "flown"] },
        { id: 38, text: "Хохотал толстенный клоун, его руки ___, ___, ___", trans: "(расти, становиться)", ans: ["grow", "grew", "grown"] },
        { id: 39, text: "Позвонить домой с работы я ___, ___, ___", trans: "(забывать)", ans: ["forget", "forgot", "forgotten"] },
        { id: 40, text: "Чтоб друзья Вас не забыли - в долг не стоит ___, ___, ___", trans: "(давать)", ans: ["give", "gave", "given"] },
        { id: 41, text: "Кто рожден как почтальон - днем и ночью ___, ___, ___", trans: "(идти, ходить, уезжать)", ans: ["go", "went", "gone"] },
        { id: 42, text: "Если на стене изъян - ты картину ___, ___, ___", trans: "(вешать, висеть)", ans: ["hang", "hung", "hung"] },
        { id: 43, text: "Ты в душе всегда поэт, если душу ___, ___, ___", trans: "(иметь)", ans: ["have", "had", "had"] },
        { id: 44, text: "Звук отправился в полёт... Это ___, ___, ___", trans: "(слышать)", ans: ["hear", "heard", "heard"] },
        { id: 45, text: "Клад грабителю не виден потому, что ___, ___, ___", trans: "(прятать)", ans: ["hide", "hid", "hidden"] },
        { id: 46, text: "Взять внаем кабриолет, разрешите - ___, ___, ___", trans: "(позволять)", ans: ["let", "let", "let"] },
        { id: 47, text: "Кто украл кабриолет? Эй, держите, ___, ___, ___!", trans: "(держать)", ans: ["hold", "held", "held"] },
        { id: 48, text: "Всем, кто любит звон монет - деньги в банке ___, ___, ___", trans: "(хранить)", ans: ["keep", "kept", "kept"] },
        { id: 49, text: "Как смешить лукавый клоун. знает ___ и ___ и ___", trans: "(знать)", ans: ["know", "knew", "known"] },
        { id: 50, text: "На снегу звериный след вас к берлоге ___, ___, ___", trans: "(вести)", ans: ["lead", "led", "led"] },
        { id: 51, text: "Вот уже который год я английский ___, ___, ___", trans: "(учить(ся))", ans: ["learn", "learnt", "learnt"] },
        { id: 52, text: "Ждет фрегат, тоскуя, кнехт... Порт корабль ___, ___, ___", trans: "(покидать)", ans: ["leave", "left", "left"] },
        { id: 53, text: "Может кто на хлеб монет мне немного ___, ___, ___?", trans: "(давать взаймы)", ans: ["lend", "lent", "lent"] },
        { id: 54, text: "Спичка звездочкой горит, если спичку ___, ___, ___", trans: "(зажигать)", ans: ["light", "lit", "lit"] },
        { id: 55, text: "Билл, держи по ветру нос - нюх опасно ___, ___, ___", trans: "(терять)", ans: ["lose", "lost", "lost"] },
        { id: 56, text: "Нам на 100 персон обед, поживее ___, ___, ___", trans: "(делать)", ans: ["make", "made", "made"] },
        { id: 57, text: "Осознания момент по-английски: ___, ___, ___", trans: "(означать)", ans: ["mean", "meant", "meant"] },
        { id: 58, text: "Без разлуки встречи нет. встреча будет: ___, ___, ___", trans: "(встречать)", ans: ["meet", "met", "met"] },
        { id: 59, text: "Был борец довольно крут - на лопатки ___, ___, ___", trans: "(класть)", ans: ["put", "put", "put"] },
        { id: 60, text: "Ты обязан с детских лет по-английски ___, ___, ___", trans: "(читать)", ans: ["read", "read", "read"] },
        { id: 61, text: "Будешь ты, как лорд, солиден, коль верхом ___, ___, ___", trans: "(ездить верхом)", ans: ["ride", "rode", "ridden"] },
        { id: 62, text: "Из парчи сияют ризы - солнце в небо ___, ___, ___", trans: "(подниматься)", ans: ["rise", "rose", "risen"] },
        { id: 63, text: "Что бы быть здоровым Вам - ежедневно ___, ___, ___", trans: "(бежать, течь)", ans: ["run", "ran", "run"] },
        { id: 64, text: "Языком вчера сосед еле-еле ___, ___, ___", trans: "(сказать)", ans: ["say", "said", "said"] },
        { id: 65, text: "Тайну мы тогда храним, коль ее не ___, ___, ___", trans: "(видеть)", ans: ["see", "saw", "seen"] },
        { id: 66, text: "Из стены я вынул болт, а картину ___, ___, ___", trans: "(продавать)", ans: ["sell", "sold", "sold"] },
        { id: 67, text: "Мы для Вас, в один момент, письма факсом ___, ___, ___", trans: "(посылать)", ans: ["send", "sent", "sent"] },
        { id: 68, text: "Мы с Марией тет-а-тет только солнце ___, ___, ___", trans: "(заходить - о солнце)", ans: ["set", "set", "set"] },
        { id: 69, text: "Эй, бармен, встряхни свой шейкер! Поживее, ___, ___, ___!", trans: "(трясти)", ans: ["shake", "shook", "shaken"] },
        { id: 70, text: "Дождь поплакал и прошел. Солнце ярко ___, ___, ___", trans: "(сиять, блестеть)", ans: ["shine", "shone", "shone"] },
        { id: 71, text: "По мишеням хорошо я прям как снайпер ___, ___, ___", trans: "(стрелять, давать побеги)", ans: ["shoot", "shot", "shot"] },
        { id: 72, text: "Люди в дом тот не спешат, там, где двери ___, ___, ___", trans: "(закрывать)", ans: ["shut", "shut", "shut"] },
        { id: 73, text: "Узнаю по голосам, кто приятно ___, ___, ___", trans: "(петь)", ans: ["sing", "sang", "sung"] },
        { id: 74, text: "Сердце тянет в небеса... И я в небо ___, ___, ___", trans: "(погружаться)", ans: ["sink", "sank", "sunk"] },
        { id: 75, text: "За семь бед один ответ... Только бы не ___, ___, ___", trans: "(сидеть)", ans: ["sit", "sat", "sat"] },
        { id: 76, text: "Сон все ближе - step by step, скоро дети ___, ___, ___", trans: "(cпать)", ans: ["sleep", "slept", "slept"] },
        { id: 77, text: "Вот уже как 40 лет детство елкой ___, ___, ___", trans: "(нюхать, пахнуть)", ans: ["smell", "smelt", "smelt"] },
        { id: 78, text: "Там, всегда не будет проку, где обильно ___, ___, ___", trans: "(говорить)", ans: ["speak", "spoke", "spoken"] },
        { id: 79, text: "Не копите много лет - жены money ___, ___, ___", trans: "(тратить)", ans: ["spend", "spent", "spent"] },
        { id: 80, text: "Стоит ли так сильно спорить? это нервы ___, ___, ___", trans: "(портить)", ans: ["spoil", "spoilt", "spoilt"] }
    ];

    let verses = [];
    let currentLevel = 0;
    const totalLevels = versesData.length;
    let hintUsed = false;

    // Fisher-Yates Shuffle Algorithm
    function shuffleArray(array) {
        const shuffled = [...array];
        for (let i = shuffled.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
        }
        return shuffled;
    }

    // DOM Elements
    const verseDisplay = document.getElementById('verse-display');
    const translationDisplay = document.getElementById('translation-display');
    const inputsContainer = document.getElementById('inputs-container');
    const checkBtn = document.getElementById('check-btn');
    const nextBtn = document.getElementById('next-btn');
    const hintBtn = document.getElementById('hint-btn');
    const restartBtn = document.getElementById('restart-btn');
    const feedback = document.getElementById('feedback');
    const currentLevelSpan = document.getElementById('current-level');
    const totalLevelsSpan = document.getElementById('total-levels');
    const progressFill = document.getElementById('progress-fill');

    function initGame() {
        // Shuffle verses on each game start
        verses = shuffleArray(versesData);
        currentLevel = 0;
        hintUsed = false;
        totalLevelsSpan.textContent = totalLevels;
        loadLevel(currentLevel);
    }

    function loadLevel(index) {
        const verse = verses[index];
        hintUsed = false;
        
        // Update Progress
        currentLevelSpan.textContent = index + 1;
        const progressPercent = ((index) / totalLevels) * 100;
        progressFill.style.width = `${progressPercent}%`;

        // Render Text
        verseDisplay.innerHTML = verse.text;
        translationDisplay.textContent = verse.trans;

        // Create Inputs with labels
        inputsContainer.innerHTML = '';
        const labels = ['1 форма', '2 форма', '3 форма'];
        for (let i = 0; i < 3; i++) {
            const wrapper = document.createElement('div');
            wrapper.className = 'input-wrapper';
            
            const label = document.createElement('span');
            label.className = 'input-label';
            label.textContent = labels[i];
            
            const input = document.createElement('input');
            input.type = 'text';
            input.placeholder = '...';
            input.dataset.index = i;
            input.addEventListener('keypress', function (e) {
                if (e.key === 'Enter') checkAnswer();
            });
            
            wrapper.appendChild(label);
            wrapper.appendChild(input);
            inputsContainer.appendChild(wrapper);
        }

        // Reset UI
        feedback.textContent = '';
        feedback.className = 'feedback-message';
        checkBtn.style.display = 'block';
        nextBtn.style.display = 'none';
        hintBtn.style.display = 'block';
        hintBtn.disabled = false;
        restartBtn.style.display = 'none';
        
        // Focus first input
        setTimeout(() => inputsContainer.children[0].querySelector('input').focus(), 100);
    }

    function showHint() {
        const inputs = inputsContainer.querySelectorAll('input');
        const currentVerse = verses[currentLevel];
        
        // Only reveal the first form
        inputs[0].value = currentVerse.ans[0];
        inputs[0].classList.add('hint-used');
        inputs[0].classList.remove('wrong', 'correct');
        
        // Keep 2nd and 3rd inputs empty for user to fill
        inputs[1].value = '';
        inputs[2].value = '';

        feedback.textContent = "Первая форма показана! Введи 2 и 3 формы сам 💪";
        feedback.className = 'feedback-message hint-msg';
        
        hintBtn.disabled = true;
        hintUsed = true;
        
        // Focus on second input
        inputs[1].focus();
    }

    function checkAnswer() {
        const inputs = inputsContainer.querySelectorAll('input');
        const currentVerse = verses[currentLevel];
        let allCorrect = true;
        let filledCount = 0;

        inputs.forEach((input, idx) => {
            const userVal = input.value.trim().toLowerCase();
            const correctVal = currentVerse.ans[idx].toLowerCase();

            if (userVal === '') {
                allCorrect = false;
            } else {
                filledCount++;
                if (userVal === correctVal) {
                    input.classList.remove('wrong', 'hint-used');
                    input.classList.add('correct');
                } else {
                    input.classList.remove('correct', 'hint-used');
                    input.classList.add('wrong');
                    allCorrect = false;
                }
            }
        });

        if (filledCount < 3) {
            feedback.textContent = "Заполни все три пропуска!";
            feedback.className = 'feedback-message error-msg';
            return;
        }

        if (allCorrect) {
            if (hintUsed) {
                feedback.textContent = "Правильно! Но ты использовал подсказку 😉";
            } else {
                feedback.textContent = "Молодец! Всё верно! 🎉";
            }
            feedback.className = 'feedback-message success-msg';
            checkBtn.style.display = 'none';
            hintBtn.style.display = 'none';
            nextBtn.style.display = 'block';
            fireConfetti();
            
            // Auto-fill the blanks in the text for visual confirmation
            let newText = verseDisplay.innerHTML;
            const blanks = newText.match(/___/g);
            if(blanks) {
                currentVerse.ans.forEach(word => {
                    newText = newText.replace('___', `<span style="color:#1dd1a1; font-weight:bold">${word}</span>`);
                });
                verseDisplay.innerHTML = newText;
            }

        } else {
            feedback.textContent = "Попробуй еще раз! Ошибки выделены.";
            feedback.className = 'feedback-message error-msg';
        }
    }

    function nextLevel() {
        currentLevel++;
        if (currentLevel < totalLevels) {
            loadLevel(currentLevel);
        } else {
            showEndScreen();
        }
    }

    function restartGame() {
        initGame();
    }

    function showEndScreen() {
        verseDisplay.innerHTML = "Поздравляем! Ты прошел все уровни! 🏆";
        translationDisplay.textContent = "Ты настоящий мастер неправильных глаголов!";
        inputsContainer.innerHTML = '';
        feedback.textContent = '';
        checkBtn.style.display = 'none';
        nextBtn.style.display = 'none';
        hintBtn.style.display = 'none';
        restartBtn.style.display = 'block';
        progressFill.style.width = '100%';
        currentLevelSpan.textContent = totalLevels;
        fireConfetti();
        fireConfetti();
    }

    // Simple Confetti Effect
    const canvas = document.getElementById('confetti-canvas');
    const ctx = canvas.getContext('2d');
    let particles = [];

    function resizeCanvas() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    function fireConfetti() {
        for (let i = 0; i < 100; i++) {
            particles.push({
                x: canvas.width / 2,
                y: canvas.height / 2,
                vx: (Math.random() - 0.5) * 10,
                vy: (Math.random() - 0.5) * 10 - 5,
                color: `hsl(${Math.random() * 360}, 100%, 50%)`,
                size: Math.random() * 5 + 2,
                life: 100
            });
        }
        requestAnimationFrame(updateConfetti);
    }

    function updateConfetti() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        particles.forEach((p, index) => {
            p.x += p.vx;
            p.y += p.vy;
            p.vy += 0.2; // gravity
            p.life--;
            p.size *= 0.96;

            ctx.fillStyle = p.color;
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
            ctx.fill();

            if (p.life <= 0 || p.size < 0.5) {
                particles.splice(index, 1);
            }
        });

        if (particles.length > 0) {
            requestAnimationFrame(updateConfetti);
        }
    }

    // Start Game
    initGame();

</script>
</body>
</html>
