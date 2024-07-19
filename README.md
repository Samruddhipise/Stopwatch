# Stopwatch
Html:
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
    <div class="display">00:00:00</div>
    <button class="control start">Start</button>
    <button class="control stop">Stop</button>
    <button class="control reset">Reset</button>
  </div>

  <script src="script.js"></script>
</body>
</html>

Css:
body {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f0f0f0;
}

.stopwatch {
  text-align: center;
}

.display {
  font-size: 2em;
  margin-bottom: 10px;
}

.control {
  font-size: 1em;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

Javascript:
// Selecting elements from the DOM
const display = document.querySelector('.display');
const startButton = document.querySelector('.start');
const stopButton = document.querySelector('.stop');
const resetButton = document.querySelector('.reset');

let startTime; // To store the start time
let elapsedTime = 0; // To store the elapsed time in milliseconds
let timerInterval; // To store the interval of the timer

function displayTime() {
  const ms = elapsedTime % 1000;
  const seconds = Math.floor(elapsedTime / 1000) % 60;
  const minutes = Math.floor(elapsedTime / (1000 * 60)) % 60;
  const hours = Math.floor(elapsedTime / (1000 * 60 * 60));

  const formattedTime = 
    `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
  
  display.textContent = formattedTime;
}

function startTimer() {
  if (!timerInterval) {
    startTime = Date.now() - elapsedTime;
    timerInterval = setInterval(function() {
      elapsedTime = Date.now() - startTime;
      displayTime();
    }, 10); // Update display every 10 milliseconds
  }
}

function stopTimer() {
  clearInterval(timerInterval);
  timerInterval = null;
}

function resetTimer() {
  clearInterval(timerInterval);
  timerInterval = null;
  elapsedTime = 0;
  displayTime();
}

// Adding event listeners to buttons
startButton.addEventListener('click', startTimer);
stopButton.addEventListener('click', stopTimer);
resetButton.addEventListener('click', resetTimer);
