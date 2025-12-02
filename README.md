<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tic-Tac-Toe</title>
    <style>
        /* CSS Styling */
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            margin: 0;
            background-color: #f4f4f9;
        }

        #game-title {
            margin-bottom: 20px;
            color: #333;
        }

        #status {
            margin-bottom: 15px;
            font-size: 1.2em;
            font-weight: bold;
            color: #555;
        }

        #board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 5px;
            background-color: #333;
            border: 5px solid #333;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .cell {
            width: 100px;
            height: 100px;
            background-color: #fff;
            border: none;
            font-size: 3em;
            cursor: pointer;
            transition: background-color 0.2s;
            outline: none;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #007bff; /* Default color for X/O */
        }

        .cell:hover:not(:disabled) {
            background-color: #e0e0e0;
        }

        .cell:disabled {
            cursor: default;
        }

        #restart-button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 1em;
            cursor: pointer;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            transition: background-color 0.3s;
        }

        #restart-button:hover {
            background-color: #218838;
        }

        /* Color styling for X and O */
        .cell.x {
            color: #dc3545; /* Red for X */
        }

        .cell.o {
            color: #007bff; /* Blue for O */
        }
    </style>
</head>
<body>
    <h1 id="game-title">Tic-Tac-Toe</h1>
    <div id="status">Player X's Turn</div>
    <div id="board">
        <button class="cell" data-index="0"></button>
        <button class="cell" data-index="1"></button>
        <button class="cell" data-index="2"></button>
        <button class="cell" data-index="3"></button>
        <button class="cell" data-index="4"></button>
        <button class="cell" data-index="5"></button>
        <button class="cell" data-index="6"></button>
        <button class="cell" data-index="7"></button>
        <button class="cell" data-index="8"></button>
    </div>
    <button id="restart-button">Restart Game</button>
    

    <script>
        // JavaScript Logic
        const cells = document.querySelectorAll('.cell');
        const statusDisplay = document.getElementById('status');
        const restartButton = document.getElementById('restart-button');
        let gameActive = true;
        let currentPlayer = 'X';
        // Array to track the state of the board, 9 empty strings initially
        let gameState = ['', '', '', '', '', '', '', '', ''];

        // Define all possible winning combinations (indices of the gameState array)
        const winningConditions = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
            [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
            [0, 4, 8], [2, 4, 6]             // Diagonals
        ];

        // --- Core Game Functions ---

        // 1. Handle a cell click
        function handleCellClick(event) {
            const clickedCell = event.target;
            const clickedCellIndex = parseInt(clickedCell.getAttribute('data-index'));

            // Check if the cell is already played or the game is over
            if (gameState[clickedCellIndex] !== '' || !gameActive) {
                return;
            }

            // Update the game state and the UI
            gameState[clickedCellIndex] = currentPlayer;
            clickedCell.innerHTML = currentPlayer;
            clickedCell.classList.add(currentPlayer.toLowerCase()); // Add class for coloring
            clickedCell.disabled = true; // Disable the button after click

            handleResultValidation();
        }

        // 2. Check for a winner or a draw
        function handleResultValidation() {
            let roundWon = false;
            for (let i = 0; i < winningConditions.length; i++) {
                const winCondition = winningConditions[i];
                let a = gameState[winCondition[0]];
                let b = gameState[winCondition[1]];
                let c = gameState[winCondition[2]];

                if (a === '' || b === '' || c === '') {
                    continue; // Skip if any cell is empty
                }
                if (a === b && b === c) {
                    roundWon = true;
                    break;
                }
            }

            if (roundWon) {
                statusDisplay.innerHTML = `Player ${currentPlayer} Wins! 🎉`;
                gameActive = false;
                // Visually highlight the winning cells (optional)
                // winningConditions.find(cond => gameState[cond[0]] === gameState[cond[1]] && gameState[cond[1]] === gameState[cond[2]]).forEach(index => {
                //     cells[index].style.backgroundColor = '#d4edda';
                // });
                return;
            }

            // Check for a Draw (if there are no more empty cells)
            let roundDraw = !gameState.includes('');
            if (roundDraw) {
                statusDisplay.innerHTML = `It's a Draw! 🤝`;
                gameActive = false;
                return;
            }

            // If no win or draw, switch players
            handlePlayerChange();
        }

        // 3. Switch to the next player
        function handlePlayerChange() {
            currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
            statusDisplay.innerHTML = `Player ${currentPlayer}'s Turn`;
        }

        // 4. Reset the game
        function handleRestartGame() {
            gameActive = true;
            currentPlayer = 'X';
            gameState = ['', '', '', '', '', '', '', '', ''];
            statusDisplay.innerHTML = `Player ${currentPlayer}'s Turn`;

            cells.forEach(cell => {
                cell.innerHTML = '';
                cell.disabled = false;
                cell.classList.remove('x', 'o');
                // cell.style.backgroundColor = '#fff'; // Reset any highlight
            });
        }

        // --- Event Listeners ---
        cells.forEach(cell => cell.addEventListener('click', handleCellClick));
        restartButton.addEventListener('click', handleRestartGame);
    </script>
</body>
</html>
