---
layout: base
title: Snake
permalink: /snake/
---

<style>
    body {
        background-color: black;
        color: white;
        font-family: Arial, sans-serif;
        text-align: center;
    }

    canvas {
        border: 2px solid white;
        margin: 20px auto;
        display: block;
        background-color: #111;
    }

    button {
        padding: 10px 20px;
        font-size: 16px;
        margin-top: 20px;
        cursor: pointer;
    }

    button:hover {
        background-color: white;
        color: black;
    }

    #score {
        font-size: 24px;
        margin-bottom: 10px;
    }
</style>

<h2>Super Cool Snake Game</h2>
<p id="score">Score: 0</p>
<canvas id="gameCanvas" width="400" height="400"></canvas>
<button id="fullscreenBtn">Fullscreen Mode</button>

<script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    const fullscreenBtn = document.getElementById("fullscreenBtn");
    const scoreDisplay = document.getElementById("score");

    const tileSize = 20;
    const rows = canvas.height / tileSize;
    const cols = canvas.width / tileSize;

    let snake = [{ x: 5, y: 5 }];
    let food = { x: 10, y: 10 };
    let direction = { x: 0, y: 0 };
    let score = 0;
    let colors = ["red", "blue", "green", "yellow", "purple", "orange"];
    let currentColor = "lime";

    // Handle keyboard input
    document.addEventListener("keydown", (e) => {
        if (e.key === "ArrowUp" && direction.y === 0) direction = { x: 0, y: -1 };
        if (e.key === "ArrowDown" && direction.y === 0) direction = { x: 0, y: 1 };
        if (e.key === "ArrowLeft" && direction.x === 0) direction = { x: -1, y: 0 };
        if (e.key === "ArrowRight" && direction.x === 0) direction = { x: 1, y: 0 };
    });

    // Fullscreen functionality
    fullscreenBtn.addEventListener("click", () => {
        if (!document.fullscreenElement) {
            canvas.requestFullscreen();
        } else {
            document.exitFullscreen();
        }
    });

    function spawnFood() {
        food = {
            x: Math.floor(Math.random() * cols),
            y: Math.floor(Math.random() * rows),
        };
    }

    function drawSnake() {
        ctx.fillStyle = currentColor;
        snake.forEach((segment) => {
            ctx.fillRect(segment.x * tileSize, segment.y * tileSize, tileSize, tileSize);
        });
    }

    function drawFood() {
        ctx.fillStyle = "red";
        ctx.fillRect(food.x * tileSize, food.y * tileSize, tileSize, tileSize);
    }

    function updateSnake() {
        const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

        // Wrap around edges
        if (head.x < 0) head.x = cols - 1;
        if (head.x >= cols) head.x = 0;
        if (head.y < 0) head.y = rows - 1;
        if (head.y >= rows) head.y = 0;

        // Check for collisions with the snake itself
        if (snake.some((segment) => segment.x === head.x && segment.y === head.y)) {
            alert("Game Over! Your score: " + score);
            snake = [{ x: 5, y: 5 }];
            direction = { x: 0, y: 0 };
            score = 0;
            scoreDisplay.textContent = "Score: " + score;
            currentColor = "lime";
            return;
        }

        snake.unshift(head);

        // Check if snake eats food
        if (head.x === food.x && head.y === food.y) {
            score++;
            scoreDisplay.textContent = "Score: " + score;
            currentColor = colors[Math.floor(Math.random() * colors.length)];
            spawnFood();
        } else {
            snake.pop();
        }
    }

    function gameLoop() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        drawSnake();
        drawFood();
        updateSnake();
    }

    spawnFood();
    setInterval(gameLoop, 150);
</script>
