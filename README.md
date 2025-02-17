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
  margin-top: 50px; /* 원하는 값으로 조정 */
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
      50% {
        transform: scale(1.1);
        opacity: 1;
      }
      100% {
        transform: scale(1.2);
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
    let successCount = 0; // 연속 성공 횟수 저장

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
      let digits = successCount >= 30 ? 6 : 5; // 30번 성공 시 6자리 숫자 생성
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
        successCount++; // 성공 횟수 증가
        alert(` 헥헥 🐶🦴            (O: ${successCount})`);
        document.getElementById("user-input").value = "";
        startGame();
      } else {
        alert("아르르르 . . . 👹");
        successCount = 0; // 실패하면 초기화
        location.reload();
      }
    }
function createFireworks() {
  const container = document.getElementById('fireworks-container');
  const numberOfParticles = 5; // 파티클 개수
  const centerX = window.innerWidth / 2; // 화면 중앙 X
  const centerY = window.innerHeight / 2; // 화면 중앙 Y

  for (let i = 0; i < numberOfParticles; i++) {
    const particle = document.createElement('div');
    particle.classList.add('firework');

    // 초기 위치를 중앙으로 설정
    particle.style.position = "absolute";
    particle.style.left = `${centerX - 25}px`; // 중앙 위치에서 약간 조정
    particle.style.top = `${centerY - 25}px`;  // 중앙 위치에서 약간 조정
    particle.style.width = "50px"; // 크기 조정
    particle.style.height = "50px";
    particle.style.backgroundImage = `url('particle.png')`;
    particle.style.backgroundSize = "cover";
    particle.style.opacity = "0.8";

    // 컨테이너에 추가
    container.appendChild(particle);

    // 랜덤한 회전 각도 적용 (0 ~ 360도)
    const rotationAngle = Math.random() * 360; // 랜덤한 회전 각도
    const rotationDirection = Math.random() > 0.5 ? 1 : -1; // 랜덤으로 회전 방향 (시계방향 또는 반시계방향)

    // 랜덤 회전을 위한 keyframes 추가
    const rotateAnimationName = `rotateAnimation${i}`; // 고유 애니메이션 이름 생성

    // 애니메이션 스타일을 동적으로 생성하여 추가
    const styleSheet = document.styleSheets[0];  // 첫 번째 스타일시트에 접근
    styleSheet.insertRule(`
      @keyframes ${rotateAnimationName} {
        0% {
          transform: rotate(0deg);
        }
        100% {
          transform: rotate(${rotationAngle * rotationDirection}deg); /* 랜덤 회전 방향 */
        }
      }
    `, styleSheet.cssRules.length);

    // 애니메이션 속성 적용 (회전 시간을 3초로 설정)
    particle.style.animation = `${rotateAnimationName} 2s ease-out`; // 3초 동안 회전

    // 랜덤한 방향 및 거리 설정
    const angle = Math.random() * Math.PI * 2; // 0 ~ 360도 방향
    const distance = Math.random() * 100 + 200; // 50~200px까지 퍼지게
    const targetX = centerX + Math.cos(angle) * distance;
    const targetY = centerY + Math.sin(angle) * distance;

    // 실제 이동 (left, top 직접 조정)
    setTimeout(() => {
      // 파티클의 이동을 위한 transition 설정
      particle.style.transition = "left 2s ease-out, top 1s ease-out, opacity 1.5s ease-in";
      particle.style.left = `${targetX}px`;
      particle.style.top = `${targetY}px`;
      particle.style.opacity = "0";

      // 중력 효과 추가: 파티클이 천천히 떨어지게 하기 위해 top을 점진적으로 늘려주기
      particle.style.transition += ", top 2s ease-out";  // 중력 효과 추가
      particle.style.top = `${targetY + 20}px`; // 떨어지게 할 거리 추가
    }, 10); // 스타일 적용을 위해 약간의 딜레이 추가

    // 2초 후 파티클 제거
    setTimeout(() => {
      particle.remove();
    }, 2000);
  }
}
  </script>
</body>
</html>
