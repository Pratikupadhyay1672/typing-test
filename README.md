# typing-test

<html lang="en">
<head>
<meta charset="UTF-8">
<title>Typing Test Website</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background:#f4f4f4;
        margin:0;
        padding:0;
    }
    .container {
        width: 60%;
        margin: 40px auto;
        background: #fff;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h1 { text-align:center; color:#333; }
    #text {
        padding:15px;
        margin:10px 0;
        font-size:18px;
        background:#eee;
        border-radius: 6px;
    }
    #inputBox {
        width:100%;
        padding:10px;
        font-size:18px;
        border:2px solid #0080ff;
        border-radius:6px;
        margin-top:10px;
    }
    #stats {
        margin-top:20px;
        display:flex;
        justify-content:space-around;
        font-size:18px;
    }
    button {
        margin-top:20px;
        padding:10px 20px;
        font-size:16px;
        background:#0080ff;
        color:white;
        border:none;
        border-radius:5px;
        cursor:pointer;
    }
</style>
</head>
<body>

<div class="container">
    <h1>Typing Speed Test</h1>

    <div id="text"></div>

    <input type="text" id="inputBox" placeholder="Start typing here..." disabled>

    <div id="stats">
        <p>Time: <span id="time">60</span>s</p>
        <p>WPM: <span id="wpm">0</span></p>
        <p>Accuracy: <span id="accuracy">0</span>%</p>
    </div>

    <button onclick="startTest()">Start Test</button>
</div>

<script>
const paragraphs = [
    "Typing speed tests help you improve your accuracy and speed over time.",
    "Practice every day to increase your typing skills and reduce mistakes.",
    "A typing test measures your words per minute and overall accuracy."
];

let timeLeft = 60;
let timer = null;
let totalTyped = 0;
let correct = 0;

function loadParagraph() {
    let randomIndex = Math.floor(Math.random() * paragraphs.length);
    document.getElementById("text").innerText = paragraphs[randomIndex];
}

function startTest() {
    loadParagraph();
    document.getElementById("inputBox").value = "";
    document.getElementById("inputBox").disabled = false;
    document.getElementById("inputBox").focus();

    timeLeft = 60;
    totalTyped = 0;
    correct = 0;

    document.getElementById("time").innerText = timeLeft;
    document.getElementById("wpm").innerText = 0;
    document.getElementById("accuracy").innerText = 0;

    if (timer) clearInterval(timer);
    timer = setInterval(updateTimer, 1000);

    document.getElementById("inputBox").addEventListener("input", calculateStats);
}

function updateTimer() {
    if (timeLeft > 0) {
        timeLeft--;
        document.getElementById("time").innerText = timeLeft;
    } else {
        clearInterval(timer);
        document.getElementById("inputBox").disabled = true;
    }
}

function calculateStats() {
    let input = document.getElementById("inputBox").value;
    let original = document.getElementById("text").innerText;

    totalTyped = input.length;

    let correctChars = 0;
    for (let i = 0; i < input.length; i++) {
        if (input[i] === original[i]) correctChars++;
    }
    correct = correctChars;

    let wpm = Math.round((correct / 5) / ((60 - timeLeft) / 60));
    if (!isFinite(wpm)) wpm = 0;

    let accuracy = Math.round((correct / totalTyped) * 100);
    if (!isFinite(accuracy)) accuracy = 0;

    document.getElementById("wpm").innerText = wpm;
    document.getElementById("accuracy").innerText = accuracy;
}
</script>

</body>
</html>
