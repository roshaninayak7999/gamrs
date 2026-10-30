# gamrs
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>All Subjects Quiz Game</title>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #141e30, #243b55);
    min-height: 100vh;
    padding: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.container {
    background: white;
    width: 100%;
    max-width: 650px;
    padding: 30px;
    border-radius: 20px;
    box-shadow: 0 10px 30px #0005;
}

h1 {
    text-align: center;
    color: #243b55;
    margin-bottom: 10px;
}

.subtitle {
    text-align: center;
    color: #666;
    margin-bottom: 25px;
}

.subjects {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
}

.subject-btn {
    background: #edf0ff;
    color: #243b55;
    padding: 18px 8px;
    border: 2px solid transparent;
    border-radius: 12px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
}

.subject-btn:hover {
    background: #d9ddff;
    border-color: #6545c5;
}

.info {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    font-weight: bold;
    gap: 10px;
}

#timer {
    color: #e63946;
}

#question {
    margin: 25px 0;
    font-size: 22px;
    color: #243b55;
}

.answers {
    display: grid;
    gap: 12px;
}

.answer-btn {
    padding: 14px;
    text-align: left;
    border: none;
    border-radius: 10px;
    background: #eee;
    font-size: 16px;
    cursor: pointer;
}

.answer-btn:hover:not(:disabled) {
    background: #dcd5ff;
}

.answer-btn:disabled {
    cursor: not-allowed;
}

.correct {
    background: #4caf50 !important;
    color: white;
}

.wrong {
    background: #e63946 !important;
    color: white;
}

.main-btn {
    width: 100%;
    padding: 14px;
    margin-top: 20px;
    border: none;
    border-radius: 10px;
    background: #6545c5;
    color: white;
    font-size: 17px;
    cursor: pointer;
}

.main-btn:hover {
    background: #4b2fa0;
}

.hidden {
    display: none;
}

#result {
    text-align: center;
    font-size: 23px;
    margin: 25px 0;
    color: #243b55;
}

@media(max-width: 450px) {
    .container {
        padding: 20px;
    }

    .info {
        font-size: 13px;
    }
}
</style>
</head>

<body>

<div class="container">

    <h1>🎯 All Subjects Quiz</h1>
    <p class="subtitle">Choose your subject and test your knowledge!</p>

    <!-- SUBJECT PAGE -->
    <div id="subject-screen">
        <div class="subjects">
            <button class="subject-btn" onclick="startQuiz('Maths')">➗ Maths</button>
            <button class="subject-btn" onclick="startQuiz('Science')">🔬 Science</button>
            <button class="subject-btn" onclick="startQuiz('English')">📚 English</button>
            <button class="subject-btn" onclick="startQuiz('History')">🏛️ History</button>
            <button class="subject-btn" onclick="startQuiz('Geography')">🌍 Geography</button>
            <button class="subject-btn" onclick="startQuiz('Computer')">💻 Computer</button>
            <button class="subject-btn" onclick="startQuiz('GK')">🧠 General Knowledge</button>
        </div>
    </div>

    <!-- QUIZ PAGE -->
    <div id="quiz-screen" class="hidden">

        <h2 id="subject-title"></h2>

        <div class="info">
            <span id="progress"></span>
            <span>Score: <span id="score">0</span></span>
            <span id="timer">20s</span>
        </div>

        <h2 id="question"></h2>

        <div class="answers" id="answers"></div>

        <button class="main-btn hidden" id="next-btn"
        onclick="nextQuestion()">Next Question</button>

    </div>

    <!-- RESULT PAGE -->
    <div id="result-screen" class="hidden">

        <h2 style="text-align:center">🏆 Quiz Completed!</h2>

        <div id="result"></div>

        <button class="main-btn" onclick="backToSubjects()">
            Choose Another Subject
        </button>

        <button class="main-btn" onclick="restartQuiz()">
            🔄 Play Again
        </button>

    </div>

</div>

<script>

// ALL SUBJECT QUESTIONS

const quizData = {

Maths: [
{
q: "What is 25 + 25?",
a: ["40", "50", "60", "70"],
c: 1
},
{
q: "What is 12 × 5?",
a: ["50", "55", "60", "65"],
c: 2
},
{
q: "What is the square of 9?",
a: ["18", "72", "81", "90"],
c: 2
},
{
q: "What is 100 ÷ 4?",
a: ["20", "25", "30", "40"],
c: 1
},
{
q: "How many sides does a triangle have?",
a: ["2", "3", "4", "5"],
c: 1
}
],

Science: [
{
q: "Which planet is known as the Red Planet?",
a: ["Earth", "Mars", "Venus", "Jupiter"],
c: 1
},
{
q: "Which gas do plants absorb?",
a: ["Oxygen", "Nitrogen", "Carbon dioxide", "Hydrogen"],
c: 2
},
{
q: "What is the chemical formula of water?",
a: ["CO2", "H2O", "O2", "NaCl"],
c: 1
},
{
q: "Which organ pumps blood?",
a: ["Brain", "Lungs", "Heart", "Kidney"],
c: 2
},
{
q: "What is the nearest star to Earth?",
a: ["Sirius", "Sun", "Polaris", "Vega"],
c: 1
}
],

English: [
{
q: "What is the plural of Child?",
a: ["Childs", "Children", "Childes", "Childrens"],
c: 1
},
{
q: "Choose the correct spelling.",
a: ["Beautifull", "Beutiful", "Beautiful", "Butiful"],
c: 2
},
{
q: "What is the opposite of Hot?",
a: ["Warm", "Cold", "Cooler", "Heat"],
c: 1
},
{
q: "Which is a noun?",
a: ["Run", "Beautiful", "Apple", "Quickly"],
c: 2
},
{
q: "What is the past tense of Go?",
a: ["Goed", "Going", "Gone", "Went"],
c: 3
}
],

History: [
{
q: "Who was the first Prime Minister of India?",
a: ["Mahatma Gandhi", "Jawaharlal Nehru", "Sardar Patel", "B. R. Ambedkar"],
c: 1
},
{
q: "When did India become independent?",
a: ["1945", "1946", "1947", "1950"],
c: 2
},
{
q: "Who was known as the Iron Man of India?",
a: ["Subhas Chandra Bose", "Sardar Patel", "Bhagat Singh", "Nehru"],
c: 1
},
{
q: "Who built the Taj Mahal?",
a: ["Akbar", "Shah Jahan", "Babur", "Aurangzeb"],
c: 1
},
{
q: "Where did the Indian Revolt of 1857 begin?",
a: ["Delhi", "Meerut", "Mumbai", "Pune"],
c: 1
}
],

Geography: [
{
q: "Which is the largest ocean?",
a: ["Atlantic", "Indian", "Pacific", "Arctic"],
c: 2
},
{
q: "Which is the highest mountain in the world?",
a: ["K2", "Mount Everest", "Kangchenjunga", "Makalu"],
c: 1
},
{
q: "What is the capital of India?",
a: ["Mumbai", "New Delhi", "Kolkata", "Chennai"],
c: 1
},
{
q: "Which continent is the largest?",
a: ["Africa", "Europe", "Asia", "Australia"],
c: 2
},
{
q: "Which river is the longest in India?",
a: ["Yamuna", "Ganga", "Godavari", "Narmada"],
c: 1
}
],

Computer: [
{
q: "What does CPU stand for?",
a: ["Central Processing Unit", "Computer Personal Unit", "Central Program Utility", "Control Processing User"],
c: 0
},
{
q: "Which language is used to style websites?",
a: ["HTML", "CSS", "Python", "C"],
c: 1
},
{
q: "What does HTML stand for?",
a: ["Hyper Text Markup Language", "High Text Machine Language", "Hyper Tool Markup Language", "Home Text Markup Language"],
c: 0
},
{
q: "Which device is used to type?",
a: ["Monitor", "Mouse", "Keyboard", "Speaker"],
c: 2
},
{
q: "Which part is called the brain of a computer?",
a: ["Mouse", "CPU", "Monitor", "Printer"],
c: 1
}
],

GK: [
{
q: "What is the national animal of India?",
a: ["Lion", "Tiger", "Elephant", "Leopard"],
c: 1
},
{
q: "How many colours are there in a rainbow?",
a: ["5", "6", "7", "8"],
c: 2
},
{
q: "Which is the largest planet?",
a: ["Earth", "Mars", "Jupiter", "Saturn"],
c: 2
},
{
q: "What is the currency of Japan?",
a: ["Dollar", "Yen", "Rupee", "Euro"],
c: 1
},
{
q: "Which is the national bird of India?",
a: ["Parrot", "Peacock", "Eagle", "Sparrow"],
c: 1
}
]

};

// GAME VARIABLES

let currentSubject = "";
let currentQuestion = 0;
let score = 0;
let timeLeft = 20;
let timerInterval;
let answered = false;

// START QUIZ

function startQuiz(subject) {

currentSubject = subject;
currentQuestion = 0;
score = 0;

document.getElementById("subject-screen").classList.add("hidden");
document.getElementById("result-screen").classList.add("hidden");
document.getElementById("quiz-screen").classList.remove("hidden");

document.getElementById("subject-title").textContent =
"📖 " + subject + " Quiz";

document.getElementById("score").textContent = score;

showQuestion();

}

// SHOW QUESTION

function showQuestion() {

clearInterval(timerInterval);

answered = false;
timeLeft = 20;

const q = quizData[currentSubject][currentQuestion];

document.getElementById("question").textContent = q.q;

document.getElementById("progress").textContent =
"Question " + (currentQuestion + 1) + " / " +
quizData[currentSubject].length;

document.getElementById("timer").textContent = "20s";

document.getElementById("next-btn").classList.add("hidden");

const answersDiv = document.getElementById("answers");

answersDiv.innerHTML = "";

q.a.forEach((answer, index) => {

const btn = document.createElement("button");

btn.textContent = answer;
btn.className = "answer-btn";

btn.onclick = () => selectAnswer(index, btn);

answersDiv.appendChild(btn);

});

timerInterval = setInterval(() => {

timeLeft--;

document.getElementById("timer").textContent = timeLeft + "s";

if (timeLeft <= 0) {
selectAnswer(-1, null);
}

}, 1000);

}

// CHECK ANSWER

function selectAnswer(index, selectedButton) {

if (answered) return;

answered = true;

clearInterval(timerInterval);

const q = quizData[currentSubject][currentQuestion];

const buttons = document.querySelectorAll(".answer-btn");

buttons.forEach((btn, i) => {

btn.disabled = true;

if (i === q.c) {
btn.classList.add("correct");
}

});

if (index === q.c) {

score++;

document.getElementById("score").textContent = score;

} else if (selectedButton) {

selectedButton.classList.add("wrong");

}

document.getElementById("next-btn").classList.remove("hidden");

}

// NEXT QUESTION

function nextQuestion() {

currentQuestion++;

if (currentQuestion < quizData[currentSubject].length) {

showQuestion();

} else {

showResult();

}

}

// SHOW RESULT

function showResult() {

clearInterval(timerInterval);

document.getElementById("quiz-screen").classList.add("hidden");

document.getElementById("result-screen").classList.remove("hidden");

let percentage = Math.round(
(score / quizData[currentSubject].length) * 100
);

document.getElementById("result").innerHTML =
"Subject: " + currentSubject +
"<br><br>Your Score: " + score + " / " +
quizData[currentSubject].length +
"<br><br>Percentage: " + percentage + "%";

}

// RESTART QUIZ

function restartQuiz() {

startQuiz(currentSubject);

}

// BACK TO SUBJECTS

function backToSubjects() {

clearInterval(timerInterval);

document.getElementById("quiz-screen").classList.add("hidden");
document.getElementById("result-screen").classList.add("hidden");
document.getElementById("subject-screen").classList.remove("hidden");

}

</script>

</body>
</html>
