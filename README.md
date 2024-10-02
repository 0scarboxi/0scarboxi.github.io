<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neon Timer</title>
  <style>
    :root {
      --colorOscar: #ffa60075; /* Aquí defines tu color en una variable */
    }

    body {
      margin: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: black;
      font-family: 'Courier New', Courier, monospace;
    }

    a{
        display: none;
    }

    .timer {
      font-size: 80px;
      color: var(--colorOscar);
      text-shadow: 0 0 20px var(--colorOscar), 0 0 30px var(--colorOscar);
    }

    button {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 18px;
      background-color: var(--colorOscar);
      border: none;
      cursor: pointer;
      color: black;
      border-radius: 10px;
    }

    button:hover {
      background-color: var(--colorOscar);
      
    }

    .container {
      text-align: center;
      
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="timer" id="timer">00:00</div>
    <button onclick="resetTimer()">Reset</button>
    <button onclick="toggleFullScreen()">Pantalla Completa</button> <!-- Nuevo botón para fullscreen -->
  </div>

  <script>
    let timerElement = document.getElementById("timer");
    let seconds = 0;
    let interval = setInterval(updateTimer, 1000);

    function updateTimer() {
      seconds++;
      let minutes = Math.floor(seconds / 60);
      let remainingSeconds = seconds % 60;
      timerElement.textContent = 
        String(minutes).padStart(2, '0') + ":" + 
        String(remainingSeconds).padStart(2, '0');
    }

    function resetTimer() {
      seconds = 0;
    }

    // Función para alternar entre pantalla completa
    function toggleFullScreen() {
      if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen(); // Entra en modo pantalla completa
      } else {
        if (document.exitFullscreen) {
          document.exitFullscreen(); // Sale de modo pantalla completa
        }
      }
    }
  </script>
</body>
</html>
