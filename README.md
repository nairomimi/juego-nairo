<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Creadora Nairobi</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      font-family: Arial, sans-serif;
    }

    canvas {
      display: block;
      background-color: #282c34;
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas"></canvas>
  <script>
    // Game setup and constants
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const player = { x: canvas.width / 2 - 25, y: canvas.height - 60, width: 50, height: 50, color: 'lime', speed: 5 };
    const obstacles = [];
    const obstacleWidth = 50, obstacleHeight = 50, obstacleSpeed = 3;
    let score = 0, isGameOver = false;
    const keys = {};

    window.addEventListener('keydown', (e) => keys[e.key] = true);
    window.addEventListener('keyup', (e) => keys[e.key] = false);

    function createObstacle() {
      obstacles.push({ x: Math.random() * (canvas.width - obstacleWidth), y: -obstacleHeight });
    }

    function update() {
      if (isGameOver) return;
      if (keys['ArrowLeft'] && player.x > 0) player.x -= player.speed;
      if (keys['ArrowRight'] && player.x < canvas.width - player.width) player.x += player.speed;

      for (let i = obstacles.length - 1; i >= 0; i--) {
        obstacles[i].y += obstacleSpeed;
        if (player.x < obstacles[i].x + obstacleWidth && player.x + player.width > obstacles[i].x &&
            player.y < obstacles[i].y + obstacleHeight && player.y + player.height > obstacles[i].y) {
          isGameOver = true;
          alert(`Game Over! Your score: ${score}`);
          window.location.reload();
        }
        if (obstacles[i].y > canvas.height) { obstacles.splice(i, 1); score++; }
      }

      if (Math.random() < 0.02) createObstacle();
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.fillStyle = player.color;
      ctx.fillRect(player.x, player.y, player.width, player.height);
      ctx.fillStyle = 'red';
      obstacles.forEach(obs => ctx.fillRect(obs.x, obs.y, obstacleWidth, obstacleHeight));
      ctx.fillStyle = 'white';
      ctx.font = '20px Arial';
      ctx.fillText(`Score: ${score}`, 10, 30);
    }

    function gameLoop() {
      update();
      draw();
      if (!isGameOver) requestAnimationFrame(gameLoop);
    }

    gameLoop();
  </script>
</body>
</html>
