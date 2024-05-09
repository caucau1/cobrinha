

<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<title>Jogo da Cobrinha</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { display: flex; justify-content: center; align-items: center; height: 100vh; background-color: #f0f0f0; }
  #game-board { width: 400px; height: 400px; background-color: #000; position: relative; }
  .snake { width: 20px; height: 20px; background-color: #00FF00; position: absolute; }
  .food { width: 20px; height: 20px; background-color: #FF0000; position: absolute; }
</style>
</head>
<body>
<div id="game-board"></div>

<script>
const board = document.getElementById('game-board');
const board_size = 400;
const cell_size = 20;
let snake = [{x: 160, y: 200}, {x: 140, y: 200}, {x: 120, y: 200}];
let food = {x: 300, y: 200};
let dx = cell_size;
let dy = 0;

function drawBoard() {
  board.innerHTML = '';
  drawSnake();
  drawFood();
}

function drawSnake() {
  snake.forEach(part => {
    const snakeElement = document.createElement('div');
    snakeElement.style.left = part.x + 'px';
    snakeElement.style.top = part.y + 'px';
    snakeElement.classList.add('snake');
    board.appendChild(snakeElement);
  });
}

function drawFood() {
  const foodElement = document.createElement('div');
  foodElement.style.left = food.x + 'px';
  foodElement.style.top = food.y + 'px';
  foodElement.classList.add('food');
  board.appendChild(foodElement);
}

function moveSnake() {
  const head = {x: snake[0].x + dx, y: snake[0].y + dy};
  snake.unshift(head);
  if (head.x === food.x && head.y === food.y) {
    placeFood();
  } else {
    snake.pop();
  }
}

function changeDirection(event) {
  const keyPressed = event.keyCode;
  const LEFT_KEY = 37;
  const RIGHT_KEY = 39;
  const UP_KEY = 38;
  const DOWN_KEY = 40;

  const goingUp = dy === -cell_size;
  const goingDown = dy === cell_size;
  const goingRight = dx === cell_size;
  const goingLeft = dx === -cell_size;

  if (keyPressed === LEFT_KEY && !goingRight) { dx = -cell_size; dy = 0; }
  if (keyPressed === UP_KEY && !goingDown) { dx = 0; dy = -cell_size; }
  if (keyPressed === RIGHT_KEY && !goingLeft) { dx = cell_size; dy = 0; }
  if (keyPressed === DOWN_KEY && !goingUp) { dx = 0; dy = cell_size; }
}

function randomFood(min, max) {
  return Math.round((Math.random() * (max-min) + min) / cell_size) * cell_size;
}

function placeFood() {
  food.x = randomFood(0, board_size - cell_size);
  food.y = randomFood(0, board_size - cell_size);
}

document.addEventListener('keydown', changeDirection);

function gameLoop() {
  setTimeout(() => {
    moveSnake();
    drawBoard();
    gameLoop();
  }, 100);
}

placeFood();
drawBoard();
gameLoop();
</script>
</body>
</html>
