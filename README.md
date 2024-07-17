<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TapSwap O'yini</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            font-family: Arial, sans-serif;
        }

        .container {
            text-align: center;
        }

        #grid {
            display: grid;
            grid-template-columns: repeat(4, 100px);
            grid-template-rows: repeat(4, 100px);
            gap: 10px;
            margin-bottom: 20px;
        }

        .cell {
            width: 100px;
            height: 100px;
            background-color: #3498db;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 24px;
            color: white;
            cursor: pointer;
            user-select: none;
            border-radius: 10px;
        }

        .active {
            background-color: #e74c3c;
        }

        #startBtn {
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
        }

        #scoreBoard {
            margin-top: 20px;
            font-size: 24px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="grid"></div>
        <button id="startBtn">O'yinni boshlash</button>
        <div id="scoreBoard">Ochko: <span id="score">0</span></div>
    </div>
    <script>
        const grid = document.getElementById('grid');
        const scoreDisplay = document.getElementById('score');
        const startBtn = document.getElementById('startBtn');
        let score = 0;
        let activeCells = [];
        let gameActive = false;

        startBtn.addEventListener('click', startGame);

        function startGame() {
            score = 0;
            scoreDisplay.textContent = score;
            gameActive = true;
            activeCells = [];
            grid.innerHTML = '';
            createGrid();
            generateActiveCells();
        }

        function createGrid() {
            for (let i = 0; i < 16; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.addEventListener('click', () => cellClicked(i));
                grid.appendChild(cell);
            }
        }

        function generateActiveCells() {
            if (!gameActive) return;
            
            for (let i = 0; i < 16; i++) {
                grid.children[i].classList.remove('active');
            }
            
            activeCells = [];
            for (let i = 0; i < 2; i++) {
                let randomIndex;
                do {
                    randomIndex = Math.floor(Math.random() * 16);
                } while (activeCells.includes(randomIndex));
                activeCells.push(randomIndex);
            }
            
            activeCells.forEach(index => {
                grid.children[index].classList.add('active');
            });
            
            setTimeout(generateActiveCells, 1000);
        }

        function cellClicked(index) {
            if (!gameActive) return;
            
            if (activeCells.includes(index)) {
                score++;
                scoreDisplay.textContent = score;
                generateActiveCells();
            } else {
                gameActive = false;
                alert('O\'yin tugadi! Yig\'ilgan ochko: ' + score);
            }
        }
    </script>
</body>
</html>

