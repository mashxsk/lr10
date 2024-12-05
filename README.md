<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Однорукий бандит 3x3</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f3f3f3;
            margin: 0;
            padding: 0;
        }

        .game {
            margin: 50px auto;
            max-width: 400px;
        }

        .slot-machine {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .slot {
            width: 100px;
            height: 100px;
            background-color: #fff;
            border: 2px solid #ddd;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .shape {
            width: 50px;
            height: 50px;
        }

        .circle {
            background-color: #f00;
            border-radius: 50%;
        }

        .square {
            background-color: #00f;
        }

        .triangle {
            width: 0;
            height: 0;
            border-left: 25px solid transparent;
            border-right: 25px solid transparent;
            border-bottom: 50px solid #0f0;
        }

        .star {
            width: 0;
            height: 0;
            border-left: 25px solid transparent;
            border-right: 25px solid transparent;
            border-bottom: 50px solid gold;
            position: relative;
        }

        .star:before {
            content: "";
            position: absolute;
            top: 15px;
            left: -25px;
            width: 0;
            height: 0;
            border-left: 25px solid transparent;
            border-right: 25px solid transparent;
            border-top: 50px solid gold;
        }

        .hexagon {
            background-color: purple;
            width: 50px;
            height: 50px;
            clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
        }
    </style>
</head>
<body>
    <div class="game">
        <h1 id="welcome-message"></h1>
        <div class="slot-machine" id="slot-machine">
            <div class="slot"></div>
            <div class="slot"></div>
            <div class="slot"></div>
            <div class="slot"></div>
            <div class="slot"></div>
            <div class="slot"></div>
            <div class="slot"></div>
            <div class="slot"></div>
            <div class="slot"></div>
        </div>
        <button id="spin-button">Крутити</button>
        <p id="message"></p>
    </div>
    <script>
        document.addEventListener("DOMContentLoaded", () => {
            const userName = prompt("Enter your name:");
            const welcomeMessage = document.getElementById("welcome-message");
            welcomeMessage.textContent = `Ласкаво просимо, ${userName}! Давайте грати!`;

            const shapes = ["circle", "square", "triangle", "star", "hexagon"];
            const slots = Array.from(document.querySelectorAll(".slot"));
            const spinButton = document.getElementById("spin-button");
            const message = document.getElementById("message");

            let attempts = 0;

            const generateRandomShape = () => shapes[Math.floor(Math.random() * shapes.length)];

            const spinSlots = () => {
                attempts++;
                const result = [];

                slots.forEach(slot => {
                    slot.innerHTML = "";
                    const shape = generateRandomShape();
                    result.push(shape);
                    const shapeDiv = document.createElement("div");
                    shapeDiv.className = `shape ${shape}`;
                    slot.appendChild(shapeDiv);
                });

                if (checkForWin(result)) {
                    message.textContent = `Вітаємо, ${userName}! Ви виграли! 🎉`;
                    spinButton.disabled = true;
                } else if (attempts >= 3) {
                    message.textContent = `Вибачте, ${userName}. Ви програли. Спробуйте ще раз!`;
                    spinButton.disabled = true;
                } else {
                    message.textContent = `Продовжуйте спроби, ${userName}! ${3 - attempts} спроб залишилось.`;
                }
            };

            const checkForWin = (result) => {
                for (let i = 0; i < 3; i++) {
                    const row = result.slice(i * 3, i * 3 + 3);
                    if (row.every(shape => shape === row[0])) return true;
                }
                return false;
            };

            spinButton.addEventListener("click", spinSlots);
        });
    </script>
</body>
</html>
