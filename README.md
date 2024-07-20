#stopwatch
To build a stopwatch web application, you can use HTML, CSS, and JavaScript. HTML is used to structure the elements of the application. By implementing functions for starting, pausing,
and resetting the stopwatch, as well as tracking and displaying lap times, users can accurately measure and record time intervals. With these technologies and functionalities, you can
create an interactive and user-friendly stopwatch web application.

HTML:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="stopwatch">
        <div class="display">
            <span id="display-hours">00</span>:<span id="display-minutes">00</span>:<span id="display-seconds">00</span>:<span id="display-centiseconds">00</span>
        </div>
        <div class="controls">
            <button id="start-btn" onclick="startStopwatch()">Start</button>
            <button id="pause-btn" onclick="pauseStopwatch()">Pause</button>
            <button id="reset-btn" onclick="resetStopwatch()">Reset</button>
            <button id="lap-btn" onclick="recordLap()">Lap</button>
        </div>
        <ul id="laps"></ul>
    </div>

    <script src="script.js"></script>
</body>
</html>

CSS:
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
}

.stopwatch {
    text-align: center;
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 300px;
}

.display {
    font-size: 2em;
    margin-bottom: 10px;
}

.controls {
    margin-bottom: 10px;
}

button {
    margin: 0 5px;
    padding: 10px 20px;
    font-size: 1em;
    cursor: pointer;
    border: none;
    border-radius: 4px;
    background-color: #4CAF50;
    color: white;
}

button:hover {
    background-color: #45a049;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin-bottom: 5px;
}

JavaScript:
let stopwatch;
let interval;
let startTime;
let elapsedTime = 0;
let laps = [];

function startStopwatch() {
    if (!interval) {
        startTime = Date.now() - elapsedTime;
        interval = setInterval(updateStopwatch, 10);
        document.getElementById('start-btn').innerText = 'Resume';
        document.getElementById('pause-btn').disabled = false;
    }
}

function pauseStopwatch() {
    clearInterval(interval);
    interval = null;
    document.getElementById('start-btn').innerText = 'Resume';
    document.getElementById('pause-btn').disabled = true;
}

function resetStopwatch() {
    clearInterval(interval);
    interval = null;
    elapsedTime = 0;
    laps = [];
    updateDisplay(0);
    document.getElementById('start-btn').innerText = 'Start';
    document.getElementById('pause-btn').disabled = true;
    clearLaps();
}

function recordLap() {
    laps.push(elapsedTime);
    let lapTime = formatTime(elapsedTime);
    let lapItem = document.createElement('li');
    lapItem.innerText = lapTime;
    document.getElementById('laps').appendChild(lapItem);
}

function updateStopwatch() {
    elapsedTime = Date.now() - startTime;
    updateDisplay(elapsedTime);
}

function updateDisplay(time) {
    let formattedTime = formatTime(time);
    document.getElementById('display-hours').innerText = formattedTime.hours;
    document.getElementById('display-minutes').innerText = formattedTime.minutes;
    document.getElementById('display-seconds').innerText = formattedTime.seconds;
    document.getElementById('display-centiseconds').innerText = formattedTime.centiseconds;
}

function formatTime(milliseconds) {
    let hours = Math.floor(milliseconds / 3600000);
    milliseconds = milliseconds % 3600000;
    let minutes = Math.floor(milliseconds / 60000);
    milliseconds = milliseconds % 60000;
    let seconds = Math.floor(milliseconds / 1000);
    let centiseconds = Math.floor(milliseconds / 10) % 100;
    
    return {
        hours: pad(hours),
        minutes: pad(minutes),
        seconds: pad(seconds),
        centiseconds: pad(centiseconds)
    };
}

function pad(number) {
    if (number < 10) {
        return '0' + number;
    }
    return number;
}

function clearLaps() {
    let lapsList = document.getElementById('laps');
    lapsList.innerHTML = '';
}

