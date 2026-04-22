<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gotham Drone Escape</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      overflow: hidden;
      font-family: 'Segoe UI', system-ui, sans-serif;
      background: #0b0e14;
    }

    .game-world {
      position: relative;
      width: 100vw;
      height: 100vh;
      background-image: url('data:image/svg+xml,%3Csvg xmlns=\'http://www.w3.org/2000/svg\' width=\'100%25\' height=\'100%25\' viewBox=\'0 0 1200 800\'%3E%3Cdefs%3E%3ClinearGradient id=\'sky\' x1=\'0\' y1=\'0\' x2=\'0\' y2=\'1\'%3E%3Cstop offset=\'0%25\' stop-color=\'%23101a2b\'/%3E%3Cstop offset=\'60%25\' stop-color=\'%231d2c42\'/%3E%3Cstop offset=\'100%25\' stop-color=\'%230c121c\'/%3E%3C/linearGradient%3E%3C/defs%3E%3Crect width=\'1200\' height=\'800\' fill=\'url(%23sky)\'/%3E%3Cpath d=\'M0 700 L1200 700 L1200 800 L0 800 Z\' fill=\'%230a0f16\'/%3E%3Crect x=\'80\' y=\'580\' width=\'30\' height=\'120\' fill=\'%23171e2c\' opacity=\'0.9\'/%3E%3Crect x=\'82\' y=\'560\' width=\'26\' height=\'26\' fill=\'%23b89b5e\' opacity=\'0.7\'/%3E%3Crect x=\'300\' y=\'620\' width=\'40\' height=\'80\' fill=\'%23171e2c\'/%3E%3Crect x=\'302\' y=\'600\' width=\'36\' height=\'22\' fill=\'%23b89b5e\' opacity=\'0.5\'/%3E%3Crect x=\'700\' y=\'560\' width=\'45\' height=\'140\' fill=\'%23171e2c\'/%3E%3Crect x=\'702\' y=\'540\' width=\'41\' height=\'24\' fill=\'%23b89b5e\' opacity=\'0.8\'/%3E%3Crect x=\'1000\' y=\'650\' width=\'25\' height=\'50\' fill=\'%23171e2c\'/%3E%3Ccircle cx=\'1100\' cy=\'180\' r=\'30\' fill=\'%23d4cdb2\' opacity=\'0.15\'/%3E%3C/svg%3E');
      background-size: cover;
      background-position: center;
      cursor: default;
      user-select: none;
    }

    .bg-element {
      position: absolute;
      width: 180px;
      height: 180px;
      top: 20vh;
      left: -200px;
      opacity: 0.25;
      z-index: 1;
      pointer-events: none;
      animation: patrolSky 18s linear infinite;
    }

    .bg-element img {
      width: 100%;
      height: 100%;
      filter: drop-shadow(0 0 12px #b89b5e55);
    }

    @keyframes patrolSky {
      0% { transform: translateX(-200px) translateY(0); }
      25% { transform: translateX(35vw) translateY(15vh); }
      50% { transform: translateX(80vw) translateY(-8vh); }
      75% { transform: translateX(40vw) translateY(25vh); }
      100% { transform: translateX(-200px) translateY(5vh); }
    }

    .hero {
      position: absolute;
      width: 120px;
      height: 120px;
      bottom: 12vh;
      left: 2vw;
      z-index: 100;
      filter: drop-shadow(0 0 14px #1e2a3a);
      transition: transform 0.4s cubic-bezier(0.25, 0.1, 0.15, 1.2);
      cursor: pointer;
      pointer-events: auto;
      transform: translateX(0);
    }

    .hero-inner {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }

    .hero img {
      width: 100%;
      height: auto;
      filter: brightness(1.1) contrast(1.2);
    }

    .hero:hover {
      transform: translateX(calc(100vw - 160px)) !important;
    }

    .obstacle {
      position: absolute;
      width: 110px;
      height: 110px;
      z-index: 50;
      pointer-events: none;
      animation-duration: 8s;
      animation-iteration-count: infinite;
      animation-timing-function: ease-in-out;
      filter: drop-shadow(0 0 6px #8b0000aa);
    }

    .drone1 {
      top: 15vh;
      left: 0;
      animation-name: flyDrone1;
    }

    @keyframes flyDrone1 {
      0% { transform: translateX(-120px) translateY(0); }
      30% { transform: translateX(45vw) translateY(12vh); }
      60% { transform: translateX(75vw) translateY(-8vh); }
      100% { transform: translateX(-120px) translateY(0); }
    }

    .drone2 {
      top: 45vh;
      right: 0;
      animation-name: flyDrone2;
    }

    @keyframes flyDrone2 {
      0% { transform: translateX(120px) translateY(0); }
      40% { transform: translateX(-30vw) translateY(-10vh); }
      70% { transform: translateX(-70vw) translateY(18vh); }
      100% { transform: translateX(120px) translateY(0); }
    }

    .drone3 {
      bottom: 5vh;
      left: 10vw;
      animation-name: flyDrone3;
    }

    @keyframes flyDrone3 {
      0% { transform: translateX(0) translateY(0); }
      25% { transform: translateX(60vw) translateY(-20vh); }
      50% { transform: translateX(30vw) translateY(5vh); }
      75% { transform: translateX(80vw) translateY(-25vh); }
      100% { transform: translateX(0) translateY(0); }
    }

    .drone-shape {
      width: 100%;
      height: 100%;
      background: radial-gradient(circle at 30% 30%, #4a3a2a, #1f1a14);
      border-radius: 45% 55% 40% 60% / 50% 40% 60% 50%;
      border: 3px solid #5e4b34;
      box-shadow: inset 0 0 10px #0a0602, 0 8px 0 #0f0b08;
      display: flex;
      align-items: center;
      justify-content: center;
      transform: rotate(10deg);
    }

    .drone-shape::before {
      content: "🛸";
      font-size: 2.5rem;
      opacity: 0.9;
      filter: grayscale(0.6);
    }

    .drone2 .drone-shape::before {
      content: "🚁";
      font-size: 3rem;
    }

    .drone3 .drone-shape::before {
      content: "🛩️";
      font-size: 2.8rem;
    }

    .hitbox {
      position: absolute;
      width: 140px;
      height: 140px;
      z-index: 80;
      pointer-events: auto;
      background: transparent;
    }

    .hitbox1 {
      top: 15vh;
      left: 0;
      animation: flyDrone1 8s infinite ease-in-out;
    }

    .hitbox2 {
      top: 45vh;
      right: 0;
      animation: flyDrone2 8s infinite ease-in-out;
    }

    .hitbox3 {
      bottom: 5vh;
      left: 10vw;
      animation: flyDrone3 9s infinite ease-in-out;
    }

    .hitbox:hover ~ .hero {
      transform: translateX(0) !important;
      transition: transform 0.3s;
    }

    .hero {
      max-width: 130px;
    }

    .hint {
      position: absolute;
      bottom: 20px;
      left: 20px;
      color: #b9c7dd;
      background: #0f1a24cc;
      padding: 8px 18px;
      border-radius: 40px;
      font-size: 1rem;
      font-weight: bold;
      backdrop-filter: blur(3px);
      border-left: 4px solid #b89b5e;
      z-index: 200;
      pointer-events: none;
    }

    .hint i {
      font-style: normal;
      display: inline-block;
      animation: pulse 1.8s infinite;
    }

    @keyframes pulse {
      0% { opacity: 0.7; }
      50% { opacity: 1; text-shadow: 0 0 5px #eed484; }
      100% { opacity: 0.7; }
    }

    .bat-logo-bg {
      position: absolute;
      bottom: 30px;
      right: 30px;
      opacity: 0.08;
      z-index: 0;
      font-size: 8rem;
      pointer-events: none;
    }
  </style>
</head>
<body>
<div class="game-world">

  <div class="bg-element">
    <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 200'%3E%3Cpath d='M100 20 L140 70 L150 60 L165 85 L185 100 L170 120 L185 145 L160 155 L145 170 L120 160 L100 180 L80 160 L55 170 L40 155 L15 145 L30 120 L15 100 L35 85 L50 60 L60 70 L100 20Z' fill='%23b89b5e' opacity='0.7' /%3E%3Ccircle cx='100' cy='100' r='45' fill='%23141a26' stroke='%23b89b5e' stroke-width='6' /%3E%3C/svg%3E" alt="bat-signal">
  </div>

  <div class="obstacle drone1">
    <div class="drone-shape"></div>
  </div>
  <div class="obstacle drone2">
    <div class="drone-shape"></div>
  </div>
  <div class="obstacle drone3">
    <div class="drone-shape"></div>
  </div>

  <div class="hitbox hitbox1"></div>
  <div class="hitbox hitbox2"></div>
  <div class="hitbox hitbox3"></div>

  <div class="hero">
    <div class="hero-inner">
      <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 200 200'%3E%3Crect width='200' height='200' fill='%231d2c3a' opacity='0.2'/%3E%3Cpath d='M100 15 L135 70 L155 60 L170 90 L190 100 L175 125 L190 155 L160 160 L150 180 L120 165 L100 185 L80 165 L50 180 L40 160 L10 155 L25 125 L10 100 L30 90 L45 60 L65 70 L100 15Z' fill='%232c4058' stroke='%23b89b5e' stroke-width='5' /%3E%3Ccircle cx='85' cy='90' r='14' fill='%230b1016' stroke='%23e4cf9e' stroke-width='3' /%3E%3Ccircle cx='115' cy='90' r='14' fill='%230b1016' stroke='%23e4cf9e' stroke-width='3' /%3E%3Cpath d='M85 125 L115 125 L108 145 L92 145 L85 125Z' fill='%23121a26' stroke='%23b89b5e' stroke-width='4' /%3E%3Cpath d='M55 75 L75 60 L95 70 L105 70 L125 60 L145 75 L130 95 L70 95 L55 75Z' fill='%231e2f40' stroke='%23b89b5e' stroke-width='4' /%3E%3Ctext x='100' y='190' font-family='Impact' font-size='20' fill='%23d4cdb2' text-anchor='middle'%3EBATMAN%3C/text%3E%3C/svg%3E" alt="Batman">
    </div>
  </div>

  <div class="hint">
    <i>Hover over Batman — he dashes right. Drones push him back!</i>
  </div>

  <div class="bat-logo-bg"></div>

</div>
</body>
</html>
