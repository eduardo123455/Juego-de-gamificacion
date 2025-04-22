<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Arquitectura a Través del Tiempo</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #f2f2f2;
      text-align: center;
      color: #333;
    }
    header {
      background-color: #a50000;
      padding: 1rem;
      color: white;
    }
    .logo {
      width: 120px;
    }
    .container {
      max-width: 900px;
      margin: auto;
      padding: 1rem;
    }
    .quiz-section, .memory-section {
      display: none;
    }
    button {
      padding: 10px 20px;
      background-color: #a50000;
      color: white;
      border: none;
      cursor: pointer;
      margin: 10px;
    }
    .card {
      width: 100px;
      height: 100px;
      margin: 10px;
      display: inline-block;
      background-color: #ccc;
      line-height: 100px;
      font-size: 24px;
      vertical-align: top;
      cursor: pointer;
    }
    .hidden {
      visibility: hidden;
    }
    .score {
      font-size: 1.2rem;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <header>
    <img src="https://upload.wikimedia.org/wikipedia/commons/3/3d/Universidad_Iberoamericana_logo.png" class="logo" alt="Logo IBERO">
    <h1>Arquitectura a Través del Tiempo</h1>
    <h3>Eduardo Cherem - Introducción a la Arquitectura<br>Profesor: Ale Bolado</h3>
  </header>

  <div class="container">
    <div id="start-section">
      <p>Bienvenido al juego. Pon a prueba tus conocimientos de Historia de la Arquitectura. ¡Haz clic para comenzar!</p>
      <button onclick="startQuiz()">Comenzar</button>
    </div>

    <div class="quiz-section" id="quiz-section">
      <div id="question"></div>
      <div id="answers"></div>
      <div class="score" id="quiz-score"></div>
    </div>

    <div class="memory-section" id="memory-section">
      <h2>Juego de Memoria Arquitectónica</h2>
      <div id="memory-board"></div>
      <div class="score" id="final-score"></div>
    </div>
  </div>

  <audio id="bg-music" loop autoplay>
    <source src="https://www.bensound.com/bensound-music/bensound-littleidea.mp3" type="audio/mp3">
  </audio>
  <audio id="correct-sound" src="https://www.soundjay.com/buttons/sounds/button-4.mp3"></audio>
  <audio id="wrong-sound" src="https://www.soundjay.com/buttons/sounds/button-10.mp3"></audio>

  <script>
    const questions = [
      {
        question: "¿Cuál es una característica del estilo Barroco?",
        answers: ["Simplicidad geométrica", "Recargamiento ornamental", "Uso del concreto", "Columnas dóricas"],
        correct: 1
      },
      {
        question: "¿Qué define al Neoclásico?",
        answers: ["Formas irregulares", "Inspiración medieval", "Inspiración en Grecia y Roma", "Uso de hormigón expuesto"],
        correct: 2
      },
      {
        question: "¿Qué caracteriza al Modernismo?",
        answers: ["Decoración excesiva", "Funcionalidad y líneas curvas", "Muros gruesos", "Fachadas con gárgolas"],
        correct: 1
      },
      {
        question: "¿Qué define al Brutalismo?",
        answers: ["Columnas dóricas", "Vidrios de colores", "Concreto expuesto y formas masivas", "Techos inclinados"] ,
        correct: 2
      }
    ];

    let currentQuestion = 0;
    let score = 0;

    function startQuiz() {
      document.getElementById("start-section").style.display = "none";
      document.getElementById("quiz-section").style.display = "block";
      showQuestion();
    }

    function showQuestion() {
      let q = questions[currentQuestion];
      document.getElementById("question").textContent = q.question;
      let answersHTML = "";
      q.answers.forEach((answer, index) => {
        answersHTML += `<button onclick="checkAnswer(${index})">${answer}</button>`;
      });
      document.getElementById("answers").innerHTML = answersHTML;
    }

    function checkAnswer(index) {
      const sound = index === questions[currentQuestion].correct ? "correct-sound" : "wrong-sound";
      document.getElementById(sound).play();
      if (index === questions[currentQuestion].correct) score++;
      currentQuestion++;
      if (currentQuestion < questions.length) {
        showQuestion();
      } else {
        document.getElementById("quiz-section").style.display = "none";
        document.getElementById("memory-section").style.display = "block";
        document.getElementById("quiz-score").textContent = `Aciertos en el quiz: ${score} de ${questions.length}`;
        startMemoryGame();
      }
    }

    // Memory game logic
    const cards = ["🏛️", "🗿", "🏗️", "🕌", "🏛️", "🗿", "🏗️", "🕌"];
    let shuffled = [];
    let selected = [];
    let matched = 0;

    function startMemoryGame() {
      shuffled = cards.sort(() => 0.5 - Math.random());
      const board = document.getElementById("memory-board");
      board.innerHTML = "";
      shuffled.forEach((icon, index) => {
        let div = document.createElement("div");
        div.className = "card";
        div.dataset.icon = icon;
        div.dataset.index = index;
        div.onclick = flipCard;
        board.appendChild(div);
      });
    }

    function flipCard() {
      if (selected.length < 2 && !this.classList.contains("matched")) {
        this.textContent = this.dataset.icon;
        selected.push(this);
        if (selected.length === 2) {
          setTimeout(checkMatch, 500);
        }
      }
    }

    function checkMatch() {
      if (selected[0].dataset.icon === selected[1].dataset.icon) {
        selected[0].classList.add("matched");
        selected[1].classList.add("matched");
        matched++;
      } else {
        selected[0].textContent = "";
        selected[1].textContent = "";
      }
      selected = [];
      if (matched === cards.length / 2) {
        document.getElementById("final-score").textContent = `Juego completado 🎉\nAciertos en el quiz: ${score} de ${questions.length}`;
      }
    }
  </script>
</body>
</html>
