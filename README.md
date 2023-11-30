# DinoRun
<!DOCTYPE html>
<html>
<head>
    <title>DinoRun</title>
    <style>
     body {
        background: rgb(1, 251, 255);
        background: -moz-linear-gradient(0deg, rgba(1, 251, 255, 1) 6%, rgba(1, 251, 255, 1) 17%, rgba(1, 251, 255, 1) 25%, rgba(15, 196, 232, 1) 33%, rgba(0, 165, 255, 1) 43%, rgba(0, 165, 255, 1) 53%, rgba(0, 165, 255, 1) 65%, rgba(0, 165, 255, 1) 74%, rgba(0, 165, 255, 1) 85%, rgba(0, 165, 255, 1) 90%);
            background: -webkit-linear-gradient(0deg, rgba(1, 251, 255, 1) 6%, rgba(1, 251, 255, 1) 17%, rgba(1, 251, 255, 1) 25%, rgba(15, 196, 232, 1) 33%, rgba(0, 165, 255, 1) 43%, rgba(0, 165, 255, 1) 53%, rgba(0, 165, 255, 1) 65%, rgba(0, 165, 255, 1) 74%, rgba(0, 165, 255, 1) 85%, rgba(0, 165, 255, 1) 90%);
            background: linear-gradient(0deg, rgba(1, 251, 255
            , 1) 6%, rgba(1, 251, 255, 1) 17%, rgba(1, 251, 255, 1) 25%, rgba(15, 196, 232, 1) 33%, rgba(0, 165, 255, 1) 43%, rgba(0, 165, 255, 1) 53%, rgba(0, 165, 255, 1) 65%, rgba(0, 165, 255, 1) 74%, rgba(0, 165, 255, 1) 85%, rgba(0, 165, 255, 1) 90%);
            filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#01fbff", endColorstr="#00a5ff", GradientType=1);
            margin: 0;
            padding: 0;
        }

        #game {
            width: 350px;
            height: 260px;
            border: 2px solid black;
            position: relative;
            overflow: hidden;
            background-image: url('https://i.ibb.co/3YrDJky/89309798-fondo-transparente-de-pixel-art-ubicaci-n-con-bosque-nevado-por-la-noche-paisaje-para-juego.jpg');
            animation: background-scroll 02s linear infinite;
        }

        @keyframes background-scroll {
            from {
                background-position: 0 0;
            }
            to {
                background-position: -1000px 0;
            }
        }

        #dinosaur {
            width: 50px;
            height: 50px;
            position: absolute;
            bottom: 0px;
            left: 40px;
            background-image: url('https://i.ibb.co/GH7x14y/be5.gif');
            background-size: contain;
            animation: run 0.8s infinite;
            transform-origin: bottom;
        }

        .obstacle {
            width: 10px;
            height: 25px;
            position: absolute;
            bottom: 0;
            left: 35px;
            background-color: red;
        }

        /* Estilos para el menú */
        #menu {
            text-align: center;
            padding: 10px;
            background-color: rgba(0, 0, 0, 0.5);
        }
        
        #menu {
            
        }

        #menu button {
            background-color: #008CBA;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            margin: 10px;
        }

        #menu button:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        h1 {
            font-family:Courier New;
        }
        p {
            font-family:Courier New;
    </style>
</head>
<body>
<div id="menu">
    <button id="startButton">Iniciar</button>
    <button id="pauseButton" disabled>Pausa</button>
</div>
<p> Hola solo te recuerdo
    presiona "Pausar" si deseas detener el juego<br> "Iniciar" para continuar</p>
<div id="game">
    <div id="dinosaur"></div>
    <div id="obstacle-container"></div>
</div>
<p id="score">Score: 0</p>

<!-- Contenedor para obstáculos -->
<div id="obstacle-container"></div>

<script>
    let dinosaur = document.getElementById("dinosaur");
    let game = document.getElementById("game");
    let obstacles = [];
    let score = 0;
    let isPlaying = false;
    let gameLoop;

    document.getElementById("startButton").addEventListener("click", function () {
        if (!isPlaying) {
            startGame();
        }
    });

    document.getElementById("pauseButton").addEventListener("click", function () {
        if (isPlaying) {
            pauseGame();
        }
    });

    function startGame() {
        isPlaying = true;
        document.getElementById("startButton").disabled = true;
        document.getElementById("pauseButton").disabled = false;

        // Restablecer el juego aquí
        resetGame();

        // Iniciar el bucle del juego
        gameLoop = setInterval(function () {
            updateObstacles();
        }, 20);
    }

    function pauseGame() {
        isPlaying = false;
        document.getElementById("startButton").disabled = false;
        document.getElementById("pauseButton").disabled = true;

        // Detener el bucle del juego
        clearInterval(gameLoop);
    }

    function resetGame() {
        // Restablecer el juego a su estado inicial
        dinosaur.style.bottom = "0px";
        score = 0;
        obstacles.forEach(function (obstacle) {
            obstacle.style.display = "none";
        });
        obstacles = [];
        document.getElementById("score").innerHTML = "Score: " + score;
    }

    function createObstacle() {
        let obstacle = document.createElement("div");
        obstacle.className = "obstacle";
        obstacle.style.left = game.offsetWidth + "px";
        document.getElementById("obstacle-container").appendChild(obstacle);
        obstacles.push(obstacle);
    }

    function updateObstacles() {
        for (let i = 0; i < obstacles.length; i++) {
            obstacles[i].style.left = obstacles[i].offsetLeft - 5 + "px";

            if (obstacles[i].offsetLeft < -obstacles[i].offsetWidth) {
                document.getElementById("obstacle-container").removeChild(obstacles[i]);
                obstacles.splice(i, 1);
                i--;
            }

            if (
                obstacles[i] &&
                obstacles[i].offsetLeft <= dinosaur.offsetLeft + dinosaur.offsetWidth &&
                obstacles[i].offsetLeft + obstacles[i].offsetWidth >= dinosaur.offsetLeft &&
                dinosaur.offsetTop + dinosaur.offsetHeight >=
                game.offsetHeight - obstacles[i].offsetHeight
            ) {
                endGame();
            }
        }

        score++;
        document.getElementById("score").innerHTML = "Score: " + score;
    }

    function endGame() {
        clearInterval(gameLoop);
        isPlaying = false;
        document.getElementById("startButton").disabled = false;
        document.getElementById("pauseButton").disabled = true;
        alert("Game over!");
    }

    function jump(height) {
        if (dinosaur.classList.contains("jump")) {
            return;
        }
        dinosaur.classList.add("jump");
        let jumpTime = height / 2;
        let jumpUp = setInterval(function () {
            let dinosaurBottom = parseInt(getComputedStyle(dinosaur).getPropertyValue("bottom"));
            if (dinosaurBottom >= height) {
                clearInterval(jumpUp);
                let fallDown = setInterval(function () {
                    let dinosaurBottom = parseInt(getComputedStyle(dinosaur).getPropertyValue("bottom"));
                    if (dinosaurBottom <= 0) {
                        clearInterval(fallDown);
                        dinosaur.classList.remove("jump");
                    } else {
                        dinosaur.style.bottom = (dinosaurBottom - 5) + "px";
                    }
                }, 20);
            } else {
                dinosaur.style.bottom = (dinosaurBottom + 5) + "px";
            }
        }, jumpTime / 15);
    }

    document.addEventListener("touchstart", function (event) {
        if (isPlaying) {
            jump(80);
        }
    });

    setInterval(function () {
        if (isPlaying) {
            createObstacle();
        }
    }, 3000);
</script>
</body>
</html>