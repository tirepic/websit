<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Irregular Verbs Game</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&display=swap" rel="stylesheet">
<style>
:root {
--primary-color: #FF9F43;
--secondary-color: #54a0ff;
--success-color: #1dd1a1;
--error-color: #ff6b6b;
--hint-color: #a55eea;
--text-color: #2f3542;
--bg-overlay: rgba(255, 255, 255, 0.92);
}
* {
-webkit-tap-highlight-color: transparent;
}
body {
font-family: 'Comic Neue', cursive, sans-serif;
margin: 0;
padding: 0;
min-height: 100vh;
display: flex;
justify-content: center;
align-items: center;
background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
background-attachment: fixed;
color: var(--text-color);
padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
}
.game-container {
background-color: var(--bg-overlay);
backdrop-filter: blur(10px);
-webkit-backdrop-filter: blur(10px);
border-radius: 25px;
padding: 1.5rem;
width: 95%;
max-width: 700px;
box-shadow: 0 10px 40px rgba(0,0,0,0.2);
text-align: center;
position: relative;
overflow: hidden;
margin: 10px;
}
header {
margin-bottom: 1.5rem;
}
h1 {
color: var(--secondary-color);
font-size: 2rem;
margin: 0;
text-shadow: 2px 2px 0px #fff;
letter-spacing: -0.5px;
}
.progress-bar {
background-color: #dfe4ea;
border-radius: 15px;
height: 20px;
width: 100%;
margin-top: 10px;
overflow: hidden;
border: 2px solid rgba(0,0,0,0.1);
}
.progress-fill {
background: linear-gradient(90deg, var(--success-color), #10ac84);
height: 100%;
width: 0%;
transition: width 0.5s ease;
border-radius: 15px;
}
.level-info {
margin-top: 8px;
font-size: 0.85rem;
color: #747d8c;
font-weight: 600;
}
.verse-card {
background: white;
border-radius: 15px;
padding: 1.5rem;
margin-bottom: 1.5rem;
box-shadow: 0 4px 15px rgba(0,0,0,0.08);
border: 3px solid #f1f2f6;
}
.verse-text {
font-size: 1.2rem;
line-height: 1.7;
margin-bottom: 1rem;
color: #57606f;
font-weight: 500;
}
.blank-space {
display: inline-block;
border-bottom: 3px dashed var(--primary-color);
color: var(--primary-color);
font-weight: bold;
min-width: 60px;
text-align: center;
margin: 0 3px;
padding: 0 5px;
}
.translation-hint {
font-size: 0.9rem;
color: #a4b0be;
margin-top: 10px;
font-style: italic;
font-weight: 500;
}
.inputs-area {
display: flex;
justify-content: center;
gap: 8px;
flex-wrap: wrap;
margin-bottom: 1.5rem;
}
.input-wrapper {
position: relative;
display: flex;
flex-direction: column;
align-items: center;
flex: 1;
min-width: 80px;
}
.input-label {
font-size: 0.75rem;
color: #747d8c;
margin-bottom: 6px;
font-weight: 600;
text-transform: uppercase;
letter-spacing: 0.5px;
}
input[type="text"] {
font-family: 'Comic Neue', cursive, sans-serif;
font-size: 1.1rem;
padding: 12px 8px;
border: 2px solid #ced6e0;
border-radius: 12px;
width: 100%;
text-align: center;
transition: all 0.3s;
outline: none;
font-weight: 700;
background: #fafafa;
}
input[type="text"]:focus {
border-color: var(--secondary-color);
box-shadow: 0 0 12px rgba(84, 160, 255, 0.4);
background: white;
transform: scale(1.02);
}
input.correct {
border-color: var(--success-color);
background-color: #e3f9e5;
color: #10ac84;
animation: pulse 0.5s ease;
}
input.wrong {
border-color: var(--error-color);
background-color: #ffeaa7;
color: #e15f41;
animation: shake 0.5s;
}
input.hint-used {
border-color: var(--hint-color);
background-color: #f3e5f5;
color: var(--hint-color);
}
@keyframes shake {
0%, 100% { transform: translateX(0); }
25% { transform: translateX(-8px); }
50% { transform: translateX(8px); }
75% { transform: translateX(-8px); }
}
@keyframes pulse {
0%, 100% { transform: scale(1); }
50% { transform: scale(1.05); }
}
.controls {
display: flex;
justify-content: center;
gap: 10px;
flex-wrap: wrap;
}
button {
font-family: 'Comic Neue', cursive, sans-serif;
font-size: 1rem;
font-weight: 700;
padding: 14px 24px;
border: none;
border-radius: 50px;
cursor: pointer;
transition: all 0.2s;
box-shadow: 0 4px 15px rgba(0,0,0,0.15);
text-transform: uppercase;
letter-spacing: 0.5px;
}
button:active {
transform: translateY(3px) scale(0.97);
box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
button:disabled {
opacity: 0.5;
cursor: not-allowed;
transform: none;
box-shadow: none;
}
.btn-check {
background: linear-gradient(135deg, var(--primary-color), #ff793f);
color: white;
}
.btn-next {
background: linear-gradient(135deg, var(--secondary-color), #2e86de);
color: white;
display: none;
}
.btn-hint {
background: linear-gradient(135deg, var(--hint-color), #8854d0);
color: white;
}
.btn-restart {
background: linear-gradient(135deg, var(--success-color), #10ac84);
color: white;
display: none;
}
.feedback-message {
min-height: 28px;
margin-top: 12px;
font-weight: 700;
font-size: 1.1rem;
padding: 8px 16px;
border-radius: 10px;
}
.success-msg { 
color: var(--success-color);
background: rgba(29, 209, 161, 0.1);
}
.error-msg { 
color: var(--error-color);
background: rgba(255, 107, 107, 0.1);
}
.hint-msg { 
color: var(--hint-color);
background: rgba(165, 94, 234, 0.1);
}
/* Confetti Canvas */
#confetti-canvas {
position: fixed;
top: 0;
left: 0;
width: 100%;
height: 100%;
pointer-events: none;
z-index: 1000;
}
/* End Screen */
.end-screen {
padding: 2rem 0;
}
.end-screen h2 {
font-size: 2rem;
color: var(--success-color);
margin-bottom: 1rem;
}
.end-screen p {
font-size: 1.2rem;
color: #57606f;
}
/* Mobile Optimizations */
@media (max-width: 480px) {
.game-container {
padding: 1rem;
border-radius: 20px;
width: 98%;
margin: 5px;
}
h1 {
font-size: 1.5rem;
}
.verse-text {
font-size: 1rem;
line-height: 1.6;
}
.verse-card {
padding: 1rem;
}
.input-label {
font-size: 0.7rem;
}
input[type="text"] {
font-size: 1rem;
padding: 10px 6px;
}
button {
font-size: 0.9rem;
padding: 12px 18px;
}
.controls {
gap: 8px;
}
.inputs-area {
gap: 6px;
}
.input-wrapper {
min-width: 70px;
}
}
@media (max-width: 350px) {
.inputs-area {
flex-direction: column;
align-items: center;
}
.input-wrapper {
width: 100%;
max-width: 200px;
}
}
</style>
</head>
<body>
<canvas id="confetti-canvas"></canvas>
<div class="game-container">
<header>
<h1>📚 Irregular Verbs</h1>
<div class="progress-bar">
<div class="progress-fill" id="progress-fill"></div>
</div>
<div class="level-info">
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
<button class="btn-hint" id="hint-btn" onclick="showHint()">💡 Подсказка</button>
<button class="btn-check" id="check-btn" onclick="checkAnswer()">✓ Проверить</button>
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
input.autocomplete = 'off';
input.autocorrect = 'off';
input.autocapitalize = 'off';
input.spellcheck = 'false';
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
setTimeout(() => {
const firstInput = inputsContainer.children[0].querySelector('input');
if (firstInput) firstInput.focus();
}, 100);
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
feedback.textContent = "⚠️ Заполни все три пропуска!";
feedback.className = 'feedback-message error-msg';
// Vibrate on mobile
if (navigator.vibrate) navigator.vibrate(100);
return;
}

if (allCorrect) {
if (hintUsed) {
feedback.textContent = "✓ Правильно! Но ты использовал подсказку 😉";
} else {
feedback.textContent = "🎉 Молодец! Всё верно!";
}
feedback.className = 'feedback-message success-msg';
checkBtn.style.display = 'none';
hintBtn.style.display = 'none';
nextBtn.style.display = 'block';
fireConfetti();

// Vibrate success on mobile
if (navigator.vibrate) navigator.vibrate([50, 50, 50]);

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
feedback.textContent = "❌ Попробуй еще раз! Ошибки выделены.";
feedback.className = 'feedback-message error-msg';
// Vibrate error on mobile
if (navigator.vibrate) navigator.vibrate(200);
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
verseDisplay.innerHTML = '<div class="end-screen"><h2>🏆 Поздравляем!</h2><p>Ты прошел все 80 уровней!</p><p style="margin-top: 1rem;">Ты настоящий мастер неправильных глаголов! 💪</p></div>';
translationDisplay.textContent = '';
inputsContainer.innerHTML = '';
feedback.textContent = '';
checkBtn.style.display = 'none';
nextBtn.style.display = 'none';
hintBtn.style.display = 'none';
restartBtn.style.display = 'block';
progressFill.style.width = '100%';
currentLevelSpan.textContent = totalLevels;
fireConfetti();
setTimeout(fireConfetti, 500);
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
for (let i = 0; i < 150; i++) {
particles.push({
x: canvas.width / 2,
y: canvas.height / 2,
vx: (Math.random() - 0.5) * 15,
vy: (Math.random() - 0.5) * 15 - 5,
color: `hsl(${Math.random() * 360}, 100%, 50%)`,
size: Math.random() * 6 + 3,
life: 120
});
}
requestAnimationFrame(updateConfetti);
}

function updateConfetti() {
ctx.clearRect(0, 0, canvas.width, canvas.height);
particles.forEach((p, index) => {
p.x += p.vx;
p.y += p.vy;
p.vy += 0.3;
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

// Prevent zoom on double tap
document.addEventListener('dblclick', function(event) {
event.preventDefault();
}, { passive: false });

// Start Game
initGame();
</script>
</body>
</html>
