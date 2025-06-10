<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tic Tac Toe</title>
  <style>
    body {
      background-color: black;
      color: white;
      font-family: Arial, sans-serif;
      text-align: center;
    }

    button {
      padding: 10px 20px;
      font-size: 20px;
      background-color: #333;
      border: none;
      color: white;
      cursor: pointer;
      margin: 10px;
    }

    button:hover {
      background-color: #555;
    }

    #board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 5px;
      margin: 20px auto;
      width: 310px;
    }

    #board div {
      width: 100px;
      height: 100px;
      background-color: #222;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 40px;
      border: 2px solid #444;
      cursor: pointer;
    }

    #board div.taken {
      cursor: not-allowed;
    }

    #scoreArea, #gameArea {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <div id="menu">
    <button id="playButton">Play</button>
    <button id="scoreButton">Score</button>
  </div>

  <div id="gameArea" style="display: none;">
    <h1>Tic Tac Toe</h1>
    <div id="board"></div>
    <button id="resetButton">Play Again</button>
    <div id="gameResult"></div>
  </div>

  <div id="scoreArea" style="display: none;">
    <h1>Game Scores</h1>
    <div id="scoreList"></div>
    <button id="backButton">Back to Menu</button>
  </div>

  <script>
    const playButton = document.getElementById("playButton");
    const scoreButton = document.getElementById("scoreButton");
    const scoreArea = document.getElementById("scoreArea");
    const gameArea = document.getElementById("gameArea");
    const backButton = document.getElementById("backButton");
    const resetButton = document.getElementById("resetButton");
    const board = document.getElementById("board");
    const scoreList = document.getElementById("scoreList");
    const gameResult = document.getElementById("gameResult");

    let player = 'X';
    let gameHistory = []; // To track win/loss
    let boardState = ['', '', '', '', '', '', '', '', ''];
    let gameOver = false;

    playButton.addEventListener("click", startGame);
    scoreButton.addEventListener("click", showScores);
    backButton.addEventListener("click", backToMenu);
    resetButton.addEventListener("click", resetGame);

    function startGame() {
      gameArea.style.display = 'block';
      document.getElementById('menu').style.display = 'none';
      initializeBoard();
      gameResult.textContent = '';
    }

    function showScores() {
      scoreArea.style.display = 'block';
      document.getElementById('menu').style.display = 'none';
      displayScores();
    }

    function backToMenu() {
      scoreArea.style.display = 'none';
      gameArea.style.display = 'none';
      document.getElementById('menu').style.display = 'block';
    }

    function initializeBoard() {
      boardState = ['', '', '', '', '', '', '', '', ''];
      gameOver = false;
      board.innerHTML = '';
      for (let i = 0; i < 9; i++) {
        const cell = document.createElement("div");
        cell.addEventListener("click", () => makeMove(i));
        board.appendChild(cell);
      }
    }

    function makeMove(index) {
      if (gameOver || boardState[index] !== '') return;
      boardState[index] = player;
      board.children[index].textContent = player;
      board.children[index].classList.add('taken');
      
      if (checkWinner()) {
        gameHistory.push(player === 'X' ? 'X Wins' : 'O Wins');
        gameResult.textContent = `${player} wins!`;
        gameOver = true;
      } else if (boardState.every(cell => cell !== '')) {
        gameHistory.push('Draw');
        gameResult.textContent = 'It\'s a draw!';
        gameOver = true;
      } else {
        player = player === 'X' ? 'O' : 'X';
      }
    }

    function checkWinner() {
      const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],
        [0, 3, 6], [1, 4, 7], [2, 5, 8],
        [0, 4, 8], [2, 4, 6]
      ];

      return winningCombinations.some(combination => {
        const [a, b, c] = combination;
        return boardState[a] && boardState[a] === boardState[b] && boardState[a] === boardState[c];
      });
    }

    function displayScores() {
      scoreList.innerHTML = '';
      if (gameHistory.length === 0) {
        scoreList.textContent = 'No games played yet.';
      } else {
        gameHistory.forEach((result, index) => {
          const scoreItem = document.createElement("div");
          scoreItem.textContent = `Game ${index + 1}: ${result}`;
          scoreList.appendChild(scoreItem);
        });
      }
    }

    function resetGame() {
      initializeBoard();
      gameResult.textContent = '';
      player = 'X';
    }
  </script>
</body>
</html>
