<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة سابوي بسيطة</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f0f0;
        }
        canvas {
            display: block;
            margin: 0 auto;
            background-color: #d3d3d3;
        }
    </style>
</head>
<body>
    <h1>لعبة سابوي بسيطة</h1>
    <canvas id="gameCanvas" width="500" height="500"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        let score = 0;
        const player = { x: 50, y: 450, width: 50, height: 50, speed: 5 };
        const obstacles = [];
        
        function startGame() {
            setInterval(updateGame, 1000 / 60);
            setInterval(generateObstacle, 2000);
        }

        function updateGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            movePlayer();
            drawPlayer();
            drawObstacles();
            checkCollisions();
            updateScore();
        }

        function movePlayer() {
            if (keys["ArrowUp"] && player.y > 0) player.y -= player.speed;
            if (keys["ArrowDown"] && player.y < canvas.height - player.height) player.y += player.speed;
            if (keys["ArrowLeft"] && player.x > 0) player.x -= player.speed;
            if (keys["ArrowRight"] && player.x < canvas.width - player.width) player.x += player.speed;
        }

        function drawPlayer() {
            ctx.fillStyle = "#00f";
            ctx.fillRect(player.x, player.y, player.width, player.height);
        }

        function generateObstacle() {
            const obstacle = { x: canvas.width, y: Math.random() * (canvas.height - 30), width: 30, height: 30 };
            obstacles.push(obstacle);
        }

        function drawObstacles() {
            obstacles.forEach(obstacle => {
                obstacle.x -= 5;
                ctx.fillStyle = "#f00";
                ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
            });
        }

        function checkCollisions() {
            obstacles.forEach(obstacle => {
                if (player.x < obstacle.x + obstacle.width && player.x + player.width > obstacle.x &&
                    player.y < obstacle.y + obstacle.height && player.y + player.height > obstacle.y) {
                    alert("Game Over! Final Score: " + score);
                }
            });
        }

        function updateScore() {
            score++;
            document.querySelector('h1').textContent = "Score: " + score;
        }

        const keys = {};
        window.addEventListener("keydown", (e) => { keys[e.key] = true; });
        window.addEventListener("keyup", (e) => { keys[e.key] = false; });

        startGame();
    </script>
</body>
</html>
