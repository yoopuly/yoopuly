<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>REVERS THE NUMBERS game</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
    }
    #game-container {
      text-align: center;
      background-color: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      position: relative;
      margin-top: 150px; /* 게임 시작 전에는 아래로 */
      transition: margin-top 0.5s ease-in-out;
    }
    #start-button, #submit-button {
      background-color: #FFB5DB;
      color: white;
      padding: 10px 20px;
      margin: 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }
    #start-button:hover, #submit-button:hover {
      background-color: #FF90CE;
    }
    #number-display {
      font-size: 2em;
      margin: 20px 0;
      color: #333;
    }
    #input-container {
      display: none;
    }
    #user-input {
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 5px;
      width: 200px;
      box-sizing: border-box;
    }
    #dog-image {
      width: 100px;
      height: auto;
      position: absolute;
      top: -60px;
      left: calc(50% - 50px);
      margin-top: -30px;
    }
    #speech-bubble {
      position: absolute;
      top: -120px;
      left: 50%;
      transform: translateX(-50%);
      width: 200px;
      padding: 10px;
      background-color: #FFFFFF;
      border-radius: 30px;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
      font-size: 14px;
      color: #333;
      text-align: center;
    }
    #fireworks-container {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 9999;
    }
    .firework {
      position: absolute;
      width: 50px;
      height: 50px;
      background-size: cover;
      animation: explode 1s forwards;
    }
    @keyframes explode {
      0% { transform: scale(0); opacity: 1; }
      50% { transform: scale(1.1); opacity: 1; }
      100% { transform: scale(1.2); opacity: 0; }
    }
  </style>
</head>
<body>
  <div id="game-container">
    <img id="dog-image" src="particle.png" alt="">
    <div id="speech-bubble">˚ 𓏸 ₊  언니 가티 게임하자 ! ‧𓂂𓏸</div>
    <button id="start-button">게임 시작</button>
    <div id="number-display"></div>
    <div id="input-container">
      <input type="text" id="user-input" placeholder="거꾸로 입력하세요">
      <button id="submit-button">맞아?</button>
    </div>
  </div>
  <div id="fireworks-container"></div>

  <script>
    let originalNumber = "";
    let successCount = 0;

    document.getElementById("start-button").addEventListener("click", startGame);
    document.getElementById("submit-button").addEventListener("click", checkAnswer);

    function startGame() {
      originalNumber = generateRandomNumber();
      document.getElementById("number-display").innerText = originalNumber;
      document.getElementById("start-button").style.display = "none";
      document.getElementById("input-container").style.display = "block";
      document.getElementById("dog-image").style.display = "none";
      document.getElementById("speech-bubble").style.display = "none";

      document.getElementById("game-container").style.marginTop = "0px";

      createFireworks();
      setTimeout(hideNumber, 2300);
    }

    function generateRandomNumber() {
      let digits = successCount >= 30 ? 6 : 5;
      let min = Math.pow(10, digits - 1);
      let max = Math.pow(10, digits) - 1;
      return Math.floor(min + Math.random() * (max - min)).toString();
    }

    function hideNumber() {
      document.getElementById("number-display").innerText = "";
    }

    function checkAnswer() {
      let userInput = document.getElementById("user-input").value;
      let reversedNumber = originalNumber.split("").reverse().join("");

      if (userInput === reversedNumber) {
        successCount++;
        alert(` 헥헥 🐶🦴 (O: ${successCount})`);
        document.getElementById("user-input").value = "";
        startGame();
      } else {
        alert("아르르르 . . . 👹");
        successCount = 0;
        location.reload();
      }
    }

    function createFireworks() {
      const container = document.getElementById('fireworks-container');
      const numberOfParticles = 5;
      const centerX = window.innerWidth / 2;
      const centerY = window.innerHeight / 2;

      for (let i = 0; i < numberOfParticles; i++) {
        const particle = document.createElement('div');
        particle.classList.add('firework');
        particle.style.left = `${centerX}px`;
        particle.style.top = `${centerY}px`;
        particle.style.backgroundImage = `url('particle.png')`;

        container.appendChild(particle);

        const angle = Math.random() * Math.PI * 2;
        const distance = Math.random() * 150 + 100;
        const targetX = centerX + Math.cos(angle) * distance;
        const targetY = centerY + Math.sin(angle) * distance;

        setTimeout(() => {
          particle.style.transition = "left 1.5s ease-out, top 1.5s ease-out, opacity 1s ease-in";
          particle.style.left = `${targetX}px`;
          particle.style.top = `${targetY}px`;
          particle.style.opacity = "0";
        }, 10);

        setTimeout(() => {
          particle.remove();
        }, 2000);
      }
    }
  </script>
</body>
</html>
