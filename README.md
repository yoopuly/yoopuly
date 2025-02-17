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
    }
    #start-button, #submit-button {
      background-color: #FFB5DB; /* 연핑크색 */
      color: white;
      padding: 10px 20px;
      margin: 10px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }
    #start-button:hover, #submit-button:hover {
      background-color: #FF90CE; /* 호버 시 조금 더 진한 연핑크색 */
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
    /* 말풍선을 중앙에 배치 및 크기, 테두리 조정 */
    #speech-bubble {
      position: absolute;
      top: -120px;
      left: 50%;
      transform: translateX(-50%);
      width: 200px;            /* 가로 크기 줄임 */
      padding: 10px;           /* 패딩 줄임 */
      background-color: #FFFFFF;
      border: 1px solid #FFFFFF; /* 테두리 두께 얇게 */
      border-radius: 30px;
      box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
      font-size: 14px;
      color: #333;
      text-align: center;
    }
    #speech-bubble:after {
      content: '';
      position: absolute;
      bottom: -10px;
      left: 50%;
      transform: translateX(-50%);
      border-width: 10px;
      border-style: solid;
      border-color: #FFFFFF transparent transparent transparent;
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
      0% {
        transform: scale(0);
        opacity: 1;
      }
      100% {
        transform: scale(1);
        opacity: 0;
      }
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

    document.getElementById("start-button").addEventListener("click", startGame);
    document.getElementById("submit-button").addEventListener("click", checkAnswer);

    function startGame() {
      originalNumber = generateRandomNumber();
      document.getElementById("number-display").innerText = originalNumber;
      document.getElementById("start-button").style.display = "none";
      document.getElementById("input-container").style.display = "block";
      document.getElementById("dog-image").style.display = "none";
      document.getElementById("speech-bubble").style.display = "none";
      createFireworks();
      setTimeout(hideNumber, 2300);
    }

    function generateRandomNumber() {
      return Math.floor(10000 + Math.random() * 90000).toString();
    }

    function hideNumber() {
      document.getElementById("number-display").innerText = "";
    }

    function checkAnswer() {
      let userInput = document.getElementById("user-input").value;
      let reversedNumber = originalNumber.split("").reverse().join("");

      if (userInput === reversedNumber) {
        alert(" 헥 헥 🐶🤍");
        document.getElementById("user-input").value = "";
        startGame();
      } else {
        alert("아르르르 . . . 🛸");
        location.reload();
      }
    }

    function createFireworks() {
      const container = document.getElementById('fireworks-container');
      const numberOfFireworks = 10;

      for (let i = 0; i < numberOfFireworks; i++) {
        const firework = document.createElement('div');
        firework.classList.add('firework');

        // 랜덤 위치 설정
        const x = Math.random() * window.innerWidth;
        const y = Math.random() * window.innerHeight;
        firework.style.left = `${x}px`;
        firework.style.top = `${y}px`;

        // 'particle.png'와 '하트.png' 중 하나를 랜덤 선택하여 적용
        const images = ['particle.png'];
        const randomImage = images[Math.floor(Math.random() * images.length)];
        firework.style.backgroundImage = `url('${randomImage}')`;

        container.appendChild(firework);

        // 애니메이션 종료 후 폭죽 제거 (3초 후)
        setTimeout(() => {
          firework.remove();
        }, 3000);
      }
    }
  </script>
</body>
</html>
