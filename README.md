<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Neon Timer</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Teko:wght@700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">

  <style>
    :root {
      --colorOscar: #ffa60075;
      --colorBlue: #0000ff; /* Definir color azul */
    }

    body {
      margin: 0;
      height: 100vh;
      display: grid;
      grid-template-rows: repeat(12, 1fr); /* 12 filas de igual tamaño */
      background-color: rgb(21, 21, 21);
      font-family: 'Teko';
    }

    .timer {
      font-size: 80px;
      color: var(--colorOscar);
      text-shadow: 0 0 20px var(--colorOscar), 0 0 30px var(--colorOscar);
      grid-row: 4 / 6; /* Ocupa las filas 4 y 5 */
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .task-list-container {
      grid-row: 6 / 10; /* Ocupa las filas 6 a 9 */
      overflow: hidden; /* Si no caben las tareas, no se ven */
      display: flex;
      justify-content: center;
    }

    ul {
      padding: 0;
      list-style-type: none;
      color: var(--colorOscar);
      font-size: 20px;
      width: 90%;
      height: 100%;
      overflow-y: auto; /* Permite scroll vertical si hay muchos registros */
      justify-self: center; /* Centra el input horizontalmente */
      text-align: center;
    }

    li {
      margin-bottom: 10px;
    }

    input {
      grid-row: 10; /* Fila 10 */
      padding: 10px;
      font-size: 18px;
      border-radius: 2px;
      border: 1px var(--colorOscar);
      width: 30%;
      height: 10%;
      justify-self: center; /* Centra el input horizontalmente */
      text-align: center;
      opacity: 20%;
    }

    .buttons {
      grid-row: 11; /* Fila 11 */
      display: flex;
      justify-content: center;
      text-align: center;
      gap: 10px;
    }

    button {
      padding: 10px 10px;
      font-size: 20px;
      background-color: var(--colorOscar);
      border: none;
      cursor: pointer;
      color: rgb(229, 229, 229);
      border-radius: 10px;
      font-family: 'Teko'; /* Asegúrate de aplicar la fuente aquí */
      height: 40px;
    }

    a,h1 {display: none;}

    .blue-flash {
      color: var(--colorBlue);
      text-shadow: 0 0 20px var(--colorBlue), 0 0 30px var(--colorBlue);
    }
  </style>
</head>
<body>
  <div class="timer" id="timer">00:00</div>

  <div class="task-list-container">
    <ul id="taskList"></ul>
  </div>

  <input type="text" id="taskInput" placeholder="Escribe una tarea y presiona Enter">

  <div class="buttons">
    <button onclick="resetTimer(confirm('Seguro que quieres borrar?'))">
      <i class="fas fa-redo"></i> Reset
    </button>
    <button onclick="toggleFullScreen()">
      <i class="fas fa-expand"></i> Pantalla Completa
    </button>
  </div>

  <script>
    let timerElement = document.getElementById("timer");
    let seconds = 0;
    let previousSeconds = 0;
    let interval = setInterval(updateTimer, 1000);
    let blinkInterval = setInterval(blinkTimer, 1200000); // Cada 20 minutos  

    function updateTimer() {
      seconds++;
      let minutes = Math.floor(seconds / 60);
      let remainingSeconds = seconds % 60;
      timerElement.textContent = 
        String(minutes).padStart(2, '0') + ":" + 
        String(remainingSeconds).padStart(2, '0');
    }

    function blinkTimer() {
      timerElement.classList.add("blue-flash");
      setTimeout(() => {
      timerElement.classList.remove("blue-flash");
    }, 10000); // Mantener el parpadeo durante 10 segundos
    }

    function resetTimer() {
      seconds = 0;
      previousSeconds = 0;
      timerElement.textContent = "00:00";
      document.getElementById("taskList").innerHTML = ""; // Borrar todos los registros
    }

    function toggleFullScreen() {
      if (!document.fullscreenElement) {
        document.documentElement.requestFullscreen();
      } else {
        if (document.exitFullscreen) {
          document.exitFullscreen();
        }
      }
    }

    const taskInput = document.getElementById("taskInput");
    const taskList = document.getElementById("taskList");

    taskInput.addEventListener("keypress", function(event) {
      if (event.key === "Enter" && taskInput.value !== "") {
        const task = taskInput.value;
        const time = timerElement.textContent;
        const timeTaken = seconds - previousSeconds;
        previousSeconds = seconds;

        const currentTime = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', hour12: false });

        // Función para formatear el tiempo en h, m, s
        function formatTime(seconds) {
          const h = Math.floor(seconds / 3600);
          const m = Math.floor((seconds % 3600) / 60);
          const s = seconds % 60;
          
          let timeString = "";
          if (h > 0) timeString += h + "h ";
          if (m > 0) timeString += m + "m ";
          if (s > 0) timeString += s + "s";
          
          return timeString.trim(); // Elimina espacios en blanco al final
        }

        const timeFormatted = formatTime(timeTaken);

        const newTaskItem = document.createElement("li");
        newTaskItem.textContent = `${task} - Uso: ${time} (inversión: ${timeFormatted}) [Hora: ${currentTime}]`;
        taskList.prepend(newTaskItem);
        taskInput.value = ""; // Limpiar el campo de texto después de enviar
      }
    });
  </script>
</body>
</html>
