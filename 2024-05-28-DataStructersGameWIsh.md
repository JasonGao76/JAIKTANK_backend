---
toc: true 
comments: false 
layout: post 
title: Datastructures Game V2
type: hacks
courses: { compsci: {week: 1} }
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Presidential Guessing Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background-color: #f4f4f4;
        }
        h1, h2 {
            color: #333;
        }
        .section {
            margin-bottom: 20px;
            padding: 15px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .input-section {
            margin-bottom: 10px;
            display: flex;
            align-items: center;
        }
        .input-section p {
            margin-right: 10px;
            font-weight: bold;
        }
        .input-section input[type="number"], .input-section button {
            padding: 10px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 5px;
            outline: none;
            margin-right: 10px;
        }
        .input-section button {
            background-color: #007BFF;
            color: #fff;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .input-section button:hover {
            background-color: #0056b3;
        }
        .result {
            margin-top: 10px;
            font-size: 18px;
        }
        .grid-container {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            margin-top: 10px;
        }
        .grid-item {
            padding: 10px;
            background-color: #007BFF;
            color: #fff;
            text-align: center;
            border-radius: 5px;
            font-size: 16px;
        }
        .grid-item:hover {
            background-color: #0056b3;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1 style="text-align: center;">Presidential Guessing Game</h1>
    <div class="section" id="guess-section">
        <h2>Guess the President's Number</h2>
        <div class="input-section">
            <p>Guess the order number of this president:</p>
            <input type="number" id="guess-input" min="1" max="46">
            <button onclick="guessPresidentNumber()">Submit Guess</button>
            <button onclick="resetGame()">Reset Game</button>
        </div>
        <p class="result" id="guess-result"></p>
    </div>
    <div class="section" id="sort-section">
        <h2>Sort the Presidents</h2>
        <div class="input-section">
            <button onclick="shufflePresidents()">Shuffle Presidents</button>
            <button onclick="sortPresidents()">Sort Presidents</button>
        </div>
        <div class="grid-container" id="shuffled-grid"></div>
        <p class="result" id="sorted-result"></p>
    </div>
    <div class="section" id="find-section">
        <h2>Find President by Number</h2>
        <div class="input-section">
            <input type="number" id="find-input" min="1" max="46">
            <button onclick="findPresidentByNumber()">Find President</button>
        </div>
        <p class="result" id="find-result"></p>
    </div>
    <script>
        const presidents = [
            "George Washington", "John Adams", "Thomas Jefferson", "James Madison",
            "James Monroe", "John Quincy Adams", "Andrew Jackson", "Martin Van Buren",
            "William Henry Harrison", "John Tyler", "James K. Polk", "Zachary Taylor",
            "Millard Fillmore", "Franklin Pierce", "James Buchanan", "Abraham Lincoln",
            "Andrew Johnson", "Ulysses S. Grant", "Rutherford B. Hayes", "James A. Garfield",
            "Chester A. Arthur", "Grover Cleveland", "Benjamin Harrison", "William McKinley",
            "Theodore Roosevelt", "William Howard Taft", "Woodrow Wilson", "Warren G. Harding",
            "Calvin Coolidge", "Herbert Hoover", "Franklin D. Roosevelt", "Harry S. Truman",
            "Dwight D. Eisenhower", "John F. Kennedy", "Lyndon B. Johnson", "Richard Nixon",
            "Gerald Ford", "Jimmy Carter", "Ronald Reagan", "George H. W. Bush",
            "Bill Clinton", "George W. Bush", "Barack Obama", "Donald Trump", "Joe Biden"
        ];
        let shuffledGrid = [];
        let currentPresident = '';
        // Initialize the game
        function initGame() {
            guessPresident();
            shufflePresidents();
        }
        // Part 1: Guess the President's Number
        function guessPresident() {
            const randomIndex = Math.floor(Math.random() * presidents.length);
            currentPresident = presidents[randomIndex];
            document.getElementById('guess-prompt').innerText = `Guess the order number of this president: ${currentPresident}`;
        }
        function guessPresidentNumber() {
            const guess = parseInt(document.getElementById('guess-input').value);
            const actualNumber = presidents.indexOf(currentPresident) + 1;
            const resultElement = document.getElementById('guess-result');
            if (guess === actualNumber) {
                resultElement.innerHTML = `<span style="color: green;">Correct!</span>`;
            } else {
                resultElement.innerHTML = `<span style="color: red;">Wrong. The correct number is ${actualNumber}.</span>`;
            }
        }
        function resetGame() {
            document.getElementById('guess-input').value = '';
            document.getElementById('guess-result').innerHTML = '';
            guessPresident();
        }
        // Part 2: Sort the Presidents
        function shufflePresidents() {
            shuffledGrid = createShuffledGrid();
            displayGrid(shuffledGrid, 'shuffled-grid');
        }
        function createShuffledGrid() {
            let tempPresidents = [...presidents];
            for (let i = tempPresidents.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [tempPresidents[i], tempPresidents[j]] = [tempPresidents[j], tempPresidents[i]];
            }
            let grid = [];
            for (let i = 0; i < 10; i++) {
                grid.push(tempPresidents.slice(i * 5, (i + 1) * 5));
            }
            return grid.flat();
        }
        function sortPresidents() {
            const sortedPresidents = bubbleSort([...shuffledGrid]);
            displayGrid(sortedPresidents, 'sorted-result');
        }
        function bubbleSort(arr) {
            const n = arr.length;
            let swapped;
            do {
                swapped = false;
                for (let i = 0; i < n - 1; i++) {
                    if (presidents.indexOf(arr[i]) > presidents.indexOf(arr[i + 1])) {
                        const temp = arr[i];
                        arr[i] = arr[i + 1];
                        arr[i + 1] = temp;
                        swapped = true;
                    }
                }
            } while (swapped);
            return arr;
        }
        function displayGrid(grid, elementId) {
            const container = document.getElementById(elementId);
            container.innerHTML = '';
            grid.forEach((pres, index) => {
                const gridItem = document.createElement('div');
                gridItem.classList.add('grid-item');
                gridItem.innerText = `${index + 1}. ${pres}`;
                container.appendChild(gridItem);
            });
        }
        // Part 3: Find President by Number
        function findPresidentByNumber() {
            const number = parseInt(document.getElementById('find-input').value);
            const resultElement = document.getElementById('find-result');
            if (number >= 1 && number <= presidents.length) {
                resultElement.innerHTML = `The president at position ${number} is <strong>${presidents[number - 1]}</strong>.`;
            } else {
                resultElement.innerHTML = `<span style="color: red;">Invalid number. Please enter a number between 1 and 46.</span>`;
            }
        }
        // Initialize the game when the page loads
        window.onload = initGame;
    </script>
</body>
</html>



