<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Juego de Triqui - Tres en Línea</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 50px;
      background: #f0f0f0;
    }
    h1 {
      margin-bottom: 20px;
    }
    #game {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 5px;
    }
    .cell {
      background-color: white;
      border: 2px solid #444;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2.5rem;
      cursor: pointer;
      user-select: none;
      transition: background-color 0.3s ease;
    }
    .cell:hover {
      background-color: #e0e0e0;
    }
    #status {
      margin-top: 20px;
      font-size: 1.2rem;
      min-height: 1.5em;
    }
    button {
      margin-top: 15px;
      padding: 10px 20px;
      font-size: 1rem;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>Triqui - Tres en Línea</h1>
  <div id="game">
    <!-- 9 celdas para el tablero -->
    <div class="cell" data-index="0"></div>
    <div class="cell" data-index="1"></div>
    <div class="cell" data-index="2"></div>
    <div class="cell" data-index="3"></div>
    <div class="cell" data-index="4"></div>
    <div class="cell" data-index="5"></div>
    <div class="cell" data-index="6"></div>
    <div class="cell" data-index="7"></div>
    <div class="cell" data-index="8"></div>
  </div>
  <div id="status"></div>
  <button id="restartBtn">Reiniciar juego</button>

  <script>
    const cells = document.querySelectorAll('.cell');
    const statusDiv = document.getElementById('status');
    const restartBtn = document.getElementById('restartBtn');

    let board = ['', '', '', '', '', '', '', '', ''];
    let currentPlayer = 'X';
    let isGameActive = true;

    const winningConditions = [
      [0, 1, 2], // filas
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6], // columnas
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8], // diagonales
      [2, 4, 6]
    ];

    function handleCellClick(e) {
      const index = e.target.getAttribute('data-index');

      if (board[index] !== '' || !isGameActive) return;

      board[index] = currentPlayer;
      e.target.textContent = currentPlayer;

      if (checkWin()) {
        statusDiv.textContent = `¡Jugador ${currentPlayer} gana! 🎉`;
        isGameActive = false;
        return;
      }

      if (board.every(cell => cell !== '')) {
        statusDiv.textContent = '¡Empate! 🤝';
        isGameActive = false;
        return;
      }

      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      statusDiv.textContent = `Turno del jugador ${currentPlayer}`;
    }

    function checkWin() {
      return winningConditions.some(condition => {
        const [a, b, c] = condition;
        return (
          board[a] === currentPlayer &&
          board[b] === currentPlayer &&
          board[c] === currentPlayer
        );
      });
    }

    function restartGame() {
      board.fill('');
      cells.forEach(cell => (cell.textContent = ''));
      currentPlayer = 'X';
      isGameActive = true;
      statusDiv.textContent = `Turno del jugador ${currentPlayer}`;
    }

    cells.forEach(cell => cell.addEventListener('click', handleCellClick));
    restartBtn.addEventListener('click', restartGame);

    // Estado inicial
    statusDiv.textContent = `Turno del jugador ${currentPlayer}`;
  </script>
</body>
</html>
