
<head>
  <meta charset="UTF-8">
  <title>Surprise Gift 🎉</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      height: 100vh;
      margin: 0;
      background: linear-gradient(135deg, #f9d423 0%, #ff4e50 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      font-family: 'Segoe UI', 'Arial', sans-serif;
    }
    .container {
      background: rgba(255,255,255,0.95);
      padding: 3rem 2.5rem;
      border-radius: 24px;
      box-shadow: 0 8px 32px rgba(0,0,0,0.15);
      text-align: center;
      position: relative;
      z-index: 1;
      animation: pop-in 1.2s cubic-bezier(.58,-0.18,.59,1.37);
    }
    h1 {
      font-size: 2.4rem;
      margin-bottom: 1rem;
      color: #ff4e50;
      text-shadow: 0 2px 10px #fff7;
      animation: fadeIn 1.8s;
    }
    p {
      font-size: 1.35rem;
      color: #333;
      letter-spacing: 0.03em;
      animation: fadeIn 2.5s;
    }
    /* Confetti animation */
    .confetti {
      position: fixed;
      top: 0; left: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 2;
    }
    @keyframes pop-in {
      0% { transform: scale(0.7) rotate(-8deg); opacity: 0; }
      70% { transform: scale(1.05) rotate(2deg); opacity: 1;}
      100% { transform: scale(1) rotate(0deg);}
    }
    @keyframes fadeIn {
      0% { opacity: 0; }
      100% { opacity: 1; }
    }
  </style>
</head>
<body>
  <canvas class="confetti"></canvas>
  <div class="container">
    <h1>🎉 Surprise! 🎉</h1>
    <p>The real gift is your amazing contribution today.<br>Thank you for making this session inspiring! <span style="font-size:1.5em;">🌟</span></p>
  </div>
  <script>
    // Simple confetti animation (vanilla JS)
    const canvas = document.querySelector('.confetti');
    const ctx = canvas.getContext('2d');
    let W = window.innerWidth, H = window.innerHeight;
    canvas.width = W;
    canvas.height = H;

    const confettiColors = ['#f9d423', '#ff4e50', '#6ee7b7', '#ffd6e0', '#fbbf24', '#c4b5fd', '#a7f3d0'];
    const confettiCount = 80;
    let confettiParticles = [];

    function randomRange(min, max) {
      return Math.random() * (max - min) + min;
    }

    function ConfettiParticle() {
      this.x = randomRange(0, W);
      this.y = randomRange(-H, 0);
      this.r = randomRange(6, 14);
      this.color = confettiColors[Math.floor(randomRange(0, confettiColors.length))];
      this.speed = randomRange(2, 5);
      this.tilt = randomRange(-10, 10);
      this.tiltAngle = randomRange(0, Math.PI * 2);
      this.tiltAngleIncrement = randomRange(0.05, 0.09);
    }

    ConfettiParticle.prototype.draw = function(ctx) {
      ctx.beginPath();
      ctx.lineWidth = this.r;
      ctx.strokeStyle = this.color;
      ctx.moveTo(this.x + this.tilt + this.r/3, this.y);
      ctx.lineTo(this.x + this.tilt, this.y + this.r*2);
      ctx.stroke();
    };

    function drawConfetti() {
      ctx.clearRect(0, 0, W, H);
      confettiParticles.forEach(particle => {
        particle.y += particle.speed;
        particle.tiltAngle += particle.tiltAngleIncrement;
        particle.tilt = Math.sin(particle.tiltAngle) * 16;

        if (particle.y > H + 30) {
          // reset to top
          particle.x = randomRange(0, W);
          particle.y = randomRange(-20, 0);
        }
        particle.draw(ctx);
      });
      requestAnimationFrame(drawConfetti);
    }

    function initConfetti() {
      confettiParticles = [];
      for (let i = 0; i < confettiCount; i++) {
        confettiParticles.push(new ConfettiParticle());
      }
    }

    window.addEventListener('resize', () => {
      W = window.innerWidth;
      H = window.innerHeight;
      canvas.width = W;
      canvas.height = H;
      initConfetti();
    });

    initConfetti();
    drawConfetti();
  </script>
</body>
</html>
