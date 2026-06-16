<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego de Multiplicación</title>
    <style>
        /* --- ESTILOS CSS --- */
        :root {
            --primary: #4a00e0;
            --secondary: #8e2de2;
            --success: #2ecc71;
            --error: #e74c3c;
            --text: #2c3e50;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, var(--secondary), var(--primary));
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
            color: var(--text);
        }

        .game-container {
            background: white;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 400px;
            text-align: center;
            box-sizing: border-box;
        }

        h1 {
            margin-top: 0;
            font-size: 24px;
            color: var(--primary);
        }

        .scoreboard {
            display: flex;
            justify-content: space-around;
            margin-bottom: 20px;
            font-weight: bold;
            font-size: 18px;
        }

        .score { color: var(--success); }
        .streak { color: #f39c12; }

        .question-box {
            background: #f8f9fa;
            border: 2px solid #e9ecef;
            border-radius: 15px;
            padding: 20px;
            font-size: 42px;
            font-weight: bold;
            margin-bottom: 20px;
            letter-spacing: 2px;
        }

        input[type="number"] {
            width: 100%;
            padding: 12px;
            font-size: 20px;
            border: 2px solid #ced4da;
            border-radius: 10px;
            outline: none;
            text-align: center;
            box-sizing: border-box;
            transition: border-color 0.2s;
        }

        input[type="number"]:focus {
            border-color: var(--primary);
        }

        /* Quitar las flechas del input de número */
        input::-webkit-outer-spin-button,
        input::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }

        button {
            width: 100%;
            background: linear-gradient(to right, var(--primary), var(--secondary));
            color: white;
            border: none;
            padding: 15px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 10px;
            cursor: pointer;
            margin-top: 15px;
            transition: transform 0.1s, opacity 0.2s;
        }

        button:active {
            transform: scale(0.98);
        }

        .feedback {
            margin-top: 15px;
            font-weight: bold;
            font-size: 18px;
            min-height: 27px; /* Evita que el contenedor salte */
        }

        .correct { color: var(--success); }
        .incorrect { color: var(--error); }
    </style>
</head>
<body>

    <div class="game-container">
        <h1>¡Desafío Matemático!</h1>
        
        <div class="scoreboard">
            <div>Puntos: <span id="score" class="score">0</span></div>
            <div>Racha: <span id="streak" class="streak">0</span> 🔥</div>
        </div>

        <div class="question-box" id="question">
            5 × 5
        </div>

        <input type="number" id="answer" placeholder="Tu respuesta aquí" autocomplete="off">
        
        <button id="btn-check">Verificar</button>
        
        <div class="feedback" id="feedback"></div>
    </div>

    <script>
        let num1, num2, correctAnswer;
        let score = 0;
        let streak = 0;

        const questionEl = document.getElementById('question');
        const answerInput = document.getElementById('answer');
        const btnCheck = document.getElementById('btn-check');
        const feedbackEl = document.getElementById('feedback');
        const scoreEl = document.getElementById('score');
        const streakEl = document.getElementById('streak');

        // Función para generar una nueva multiplicación (Tablas del 2 al 10)
        function generateQuestion() {
            num1 = Math.floor(Math.random() * 9) + 2; 
            num2 = Math.floor(Math.random() * 9) + 2; 
            correctAnswer = num1 * num2;
            
            questionEl.textContent = `${num1} × ${num2}`;
            answerInput.value = '';
            answerInput.focus();
        }

        // Función para validar la respuesta
        function checkAnswer() {
            const userAnswer = parseInt(answerInput.value);

            if (isNaN(userAnswer)) {
                feedbackEl.textContent = "🤔 Por favor, escribe un número.";
                feedbackEl.className = "feedback";
                return;
            }

            if (userAnswer === correctAnswer) {
                score += 10;
                streak++;
                feedbackEl.textContent = "¡Correcto! 🎉";
                feedbackEl.className = "feedback correct";
            } else {
                streak = 0; // Rompe la racha
                feedbackEl.textContent = `Incorrecto 😢 Era ${correctAnswer}`;
                feedbackEl.className = "feedback incorrect";
            }

            // Actualizar marcador
            scoreEl.textContent = score;
            streakEl.textContent = streak;

            // Esperar 1.5 segundos y pasar a la siguiente pregunta
            btnCheck.disabled = true;
            setTimeout(() => {
                feedbackEl.textContent = "";
                btnCheck.disabled = false;
                generateQuestion();
            }, 1500);
        }

        // Eventos para jugar con el botón o presionando "Enter"
        btnCheck.addEventListener('click', checkAnswer);
        answerInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') checkAnswer();
        });

        // Iniciar el juego al cargar la página
        generateQuestion();
    </script>
</body>
</html>
  
