<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch App</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>

    <div class="stopwatch">
        <h1>Stopwatch</h1>
        <div id="display">00:00:00</div>
        <div class="buttons">
            <button id="startPauseBtn">Start</button>
            <button id="resetBtn">Reset</button>
            <button id="lapBtn">Lap</button>
        </div>
        <ul id="laps"></ul>
    </div>

    <script src="script.js"></script>
</body>
</html>

/* General Styles */
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: #f4f4f4;
    margin: 0;
}

/* Stopwatch Container */
.stopwatch {
    text-align: center;
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

/* Display Timer */
#display {
    font-size: 40px;
    font-weight: bold;
    margin: 20px 0;
}

/* Buttons */
.buttons button {
    padding: 10px 20px;
    font-size: 16px;
    margin: 5px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: 0.3s;
}

#startPauseBtn { background: green; color: white; }
#resetBtn { background: red; color: white; }
#lapBtn { background: blue; color: white; }

.buttons button:hover {
    opacity: 0.8;
}

/* Lap Times */
#laps {
    list-style: none;
    padding: 0;
    margin-top: 20px;
}

#laps li {
    background: #ddd;
    padding: 10px;
    margin: 5px 0;
    border-radius: 5px;
}

let timer;
let isRunning = false;
let startTime = 0;
let elapsedTime = 0;

const display = document.getElementById("display");
const startPauseBtn = document.getElementById("startPauseBtn");
const resetBtn = document.getElementById("resetBtn");
const lapBtn = document.getElementById("lapBtn");
const laps = document.getElementById("laps");

// Function to start or pause the stopwatch
startPauseBtn.addEventListener("click", () => {
    if (!isRunning) {
        startTime = Date.now() - elapsedTime;
        timer = setInterval(updateTime, 1000);
        startPauseBtn.textContent = "Pause";
        startPauseBtn.style.background = "orange";
        isRunning = true;
    } else {
        clearInterval(timer);
        elapsedTime = Date.now() - startTime;
        startPauseBtn.textContent = "Start";
        startPauseBtn.style.background = "green";
        isRunning = false;
    }
});

// Function to reset the stopwatch
resetBtn.addEventListener("click", () => {
    clearInterval(timer);
    isRunning = false;
    startTime = 0;
    elapsedTime = 0;
    display.textContent = "00:00:00";
    startPauseBtn.textContent = "Start";
    startPauseBtn.style.background = "green";
    laps.innerHTML = "";
});

// Function to update the display time
function updateTime() {
    elapsedTime = Date.now() - startTime;
    let totalSeconds = Math.floor(elapsedTime / 1000);
    let hours = Math.floor(totalSeconds / 3600);
    let minutes = Math.floor((totalSeconds % 3600) / 60);
    let seconds = totalSeconds % 60;

    display.textContent = 
        `${String(hours).padStart(2, '0')}:${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
}

// Function to record lap times
lapBtn.addEventListener("click", () => {
    if (isRunning) {
        let lapTime = display.textContent;
        let lapItem = document.createElement("li");
        lapItem.textContent = `Lap ${laps.children.length + 1}: ${lapTime}`;
        laps.appendChild(lapItem);
    }
});
