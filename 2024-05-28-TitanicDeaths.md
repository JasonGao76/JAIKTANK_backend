<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Emergency Response System and Dice Game</title>
    <style>
        body {
            background-color: blue;
            font-family: 'Times New Roman', Times, serif;
            color: darkblue;
            padding: 20px;
        }
        .container {
            background-color: lightblue;
            color: darkblue;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }
        h1 {
            font-family: cursive;
        }
        #output, #diceOutput, #diceThrowOutput {
            margin: 20px 0;
        }
        .dice {
            display: inline-block;
            width: 50px;
            height: 50px;
            background-color: black;
            margin: 10px;
            position: relative;
        }
        .dot {
            width: 10px;
            height: 10px;
            background-color: white;
            border-radius: 50%;
            position: absolute;
        }
        .dot1 { top: 50%; left: 50%; transform: translate(-50%, -50%); }
        .dot2 { top: 20%; left: 20%; }
        .dot3 { top: 20%; right: 20%; }
        .dot4 { bottom: 20%; left: 20%; }
        .dot5 { bottom: 20%; right: 20%; }
        .dot6 { top: 50%; left: 20%; transform: translateY(-50%); }
        .dot7 { top: 50%; right: 20%; transform: translateY(-50%); }
    </style>
    <script>
        class Passenger {
            constructor(name, age, gender, ticket_class, survived) {
                this.name = name;
                this.age = age;
                this.gender = gender;
                this.ticket_class = ticket_class;
                this.survived = survived;
            }
        }
        class EmergencyResponseSystem {
            constructor(passengers) {
                this.passengers = passengers;
                this.passengers.sort((a, b) => a.name.localeCompare(b.name));
            }
            binarySearch(passengerName) {
                let low = 0;
                let high = this.passengers.length - 1;
                while (low <= high) {
                    const mid = Math.floor((low + high) / 2);
                    const currentName = this.passengers[mid].name;
                    if (currentName === passengerName) {
                        return this.passengers[mid];
                    } else if (currentName < passengerName) {
                        low = mid + 1;
                    } else {
                        high = mid - 1;
                    }
                }
                return null;
            }
            searchPassengerByName(passengerName) {
                return this.binarySearch(passengerName);
            }
            isSafeOnLifeboat(passengerName) {
                const passenger = this.searchPassengerByName(passengerName);
                if (passenger) {
                    return passenger.age < 18 || passenger.gender === 'female' || passenger.ticket_class === 1;
                }
                return false;
            }
            didPassengerSurvive(passengerName) {
                const passenger = this.searchPassengerByName(passengerName);
                return passenger ? passenger.survived : null;
            }
        }
        const passengerData = [
            new Passenger("John Smith", 25, "male", 3, false),
            new Passenger("Jane Doe", 30, "female", 1, true),
            new Passenger("Alice Johnson", 10, "female", 2, true)
        ];
        const emergencySystem = new EmergencyResponseSystem(passengerData);
        function handleSearch() {
            const passengerNameInput = document.getElementById('passengerNameInput').value;
            const outputDiv = document.getElementById('output');
            outputDiv.innerHTML = '';
            const didPassengerSurvive = emergencySystem.didPassengerSurvive(passengerNameInput);
            if (didPassengerSurvive !== null) {
                outputDiv.innerHTML = `<p>${passengerNameInput} ${didPassengerSurvive ? 'survived' : 'did not survive'}.</p>`;
            } else {
                outputDiv.innerHTML = `<p>Passenger not found.</p>`;
            }
        }
        function tossDice() {
            return [Math.floor(Math.random() * 6) + 1, Math.floor(Math.random() * 6) + 1];
        }
        function displayDice(dice, container) {
            container.innerHTML = '';
            dice.forEach(die => {
                const diceDiv = document.createElement('div');
                diceDiv.classList.add('dice');
                switch (die) {
                    case 1:
                        diceDiv.innerHTML = '<div class="dot dot1"></div>';
                        break;
                    case 2:
                        diceDiv.innerHTML = '<div class="dot dot2"></div><div class="dot dot5"></div>';
                        break;
                    case 3:
                        diceDiv.innerHTML = '<div class="dot dot2"></div><div class="dot dot1"></div><div class="dot dot5"></div>';
                        break;
                    case 4:
                        diceDiv.innerHTML = '<div class="dot dot2"></div><div class="dot dot3"></div><div class="dot dot4"></div><div class="dot dot5"></div>';
                        break;
                    case 5:
                        diceDiv.innerHTML = '<div class="dot dot2"></div><div class="dot dot3"></div><div class="dot dot4"></div><div class="dot dot5"></div><div class="dot dot1"></div>';
                        break;
                    case 6:
                        diceDiv.innerHTML = '<div class="dot dot2"></div><div class="dot dot3"></div><div class="dot dot4"></div><div class="dot dot5"></div><div class="dot dot6"></div><div class="dot dot7"></div>';
                        break;
                }
                container.appendChild(diceDiv);
            });
        }
        function runSimulations(numTrials) {
            let wins = 0;
            const diceOutputDiv = document.getElementById('diceOutput');
            const diceContainer = document.createElement('div');
            diceOutputDiv.innerHTML = '';
            diceOutputDiv.appendChild(diceContainer);
            for (let i = 0; i < numTrials; i++) {
                const dice = tossDice();
                if (dice[0] === dice[1]) {
                    wins += 1;
                }
            }
            if (wins > 0) {
                diceOutputDiv.innerHTML += `Congratulations! You won by rolling doubles.<br>`;
            } else {
                diceOutputDiv.innerHTML += `No doubles rolled. Try again!<br>`;
            }
            displayDice([6, 6], diceContainer);  // Displaying two sixes as an example
        }
        function throwDice() {
            const dice = tossDice();
            const diceOutputDiv = document.getElementById('diceThrowOutput');
            const diceContainer = document.createElement('div');
            diceOutputDiv.innerHTML = 'You rolled:<br>';
            diceOutputDiv.appendChild(diceContainer);
            displayDice(dice, diceContainer);
        }
        document.addEventListener('DOMContentLoaded', (event) => {
            runSimulations(1); // Run single simulation for user's throw
        });
    </script>
</head>
<body>
    <div class="container">
        <h1 style="font-family: cursive;">Did they survive the Titanic?</h1>
        <label for="passengerNameInput">Enter passenger name:</label>
        <input type="text" id="passengerNameInput">
        <button onclick="handleSearch()">Search</button>
        <div id="output"></div>
    </div>
    <div class="container">
        <h1 style="font-family: cursive;">John's Last Game</h1>
        <div id="diceOutput"></div>
        <div id="diceThrowOutput"></div>
        <button onclick="throwDice()">Throw Dice</button>
    </div>
</body>
</html>