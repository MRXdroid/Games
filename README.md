# Games
Juegos
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Juego del Pez Mejorado</title>
  <style>
    body { margin: 0; overflow: hidden; background: #00bcd4; font-family: Arial, sans-serif; }
    #game {
      position: relative;
      width: 100vw;
      height: 100vh;
      overflow: hidden;
    }
    #fish {
      position: absolute;
      width: 60px;
      height: 40px;
      background: orange;
      border-radius: 50%;
      top: 50%;
      left: 50px;
      transform: translateY(-50%);
    }
    .rock {
      position: absolute;
      width: 40px;
      height: 40px;
      background: gray;
      border-radius: 50%;
      top: -40px;
    }
    #score {
      position: absolute;
      top: 10px;
      right: 20px;
      font-size: 24px;
      color: white;
    }
  </style>
</head>
<body>
  <div id="game">
    <div id="fish"></div>
    <div id="score">Puntos: 0</div>
    <audio id="bgm" src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" autoplay loop></audio>
    <audio id="failSound" src="https://www.myinstants.com/media/sounds/mario-death.mp3"></audio>
  </div>

  <script>
    const game = document.getElementById('game');
    const fish = document.getElementById('fish');
    const scoreDisplay = document.getElementById('score');
    const bgm = document.getElementById('bgm');
    const failSound = document.getElementById('failSound');
    let fishY = window.innerHeight / 2;
    let score = 0;
    let speed = 5;

    document.addEventListener('keydown', e => {
      if (e.key === 'ArrowUp') fishY -= 30;
      if (e.key === 'ArrowDown') fishY += 30;
      if (fishY < 0) fishY = 0;
      if (fishY > window.innerHeight - 40) fishY = window.innerHeight - 40;
      fish.style.top = `${fishY}px`;
    });

    function createRock() {
      const rock = document.createElement('div');
      rock.classList.add('rock');
      rock.style.left = `${Math.random() * (window.innerWidth - 40)}px`;
      game.appendChild(rock);

      let rockY = -40;
      const interval = setInterval(() => {
        rockY += speed;
        rock.style.top = `${rockY}px`;

        const fishRect = fish.getBoundingClientRect();
        const rockRect = rock.getBoundingClientRect();
        if (
          fishRect.left < rockRect.right &&
          fishRect.right > rockRect.left &&
          fishRect.top < rockRect.bottom &&
          fishRect.bottom > rockRect.top
        ) {
          failSound.play();
          bgm.pause();
          alert(`¡Perdiste! Puntaje final: ${score}`);
          window.location.reload();
        }

        if (rockY > window.innerHeight) {
          rock.remove();
          clearInterval(interval);
        }
      }, 30);
    }

    // Incrementar puntuación y dificultad
    setInterval(() => {
      score++;
      scoreDisplay.textContent = `Puntos: ${score}`;
      if (score % 10 === 0 && speed < 20) {
        speed += 0.5;
      }
    }, 1000);

    setInterval(createRock, 1000);
  </script>
</body>
</html>
