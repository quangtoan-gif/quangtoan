# index.html
<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Trung Thu Vui Vẻ 🎑 | Quang Toàn Studio</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      background: radial-gradient(circle at bottom, #090a0f 0%, #000 100%);
      color: #fff;
      font-family: 'Poppins', sans-serif;
    }

    .container {
      position: relative;
      width: 100vw;
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      text-align: center;
      z-index: 2;
      opacity: 0;
      animation: fadeIn 3s ease-in-out forwards;
    }

    @keyframes fadeIn {
      0% { opacity: 0; transform: translateY(40px); }
      100% { opacity: 1; transform: translateY(0); }
    }

    h1 {
      font-size: 3em;
      background: linear-gradient(90deg, #ffcc70, #ff6bcb, #7afcff);
      -webkit-background-clip: text;
      color: transparent;
      animation: glow 2s infinite alternate;
    }

    p {
      font-size: 1.4em;
      color: #fff;
      opacity: 0.9;
    }

    @keyframes glow {
      from { text-shadow: 0 0 5px #ff6bcb; }
      to { text-shadow: 0 0 20px #ffd86b; }
    }

    .emoji {
      position: absolute;
      user-select: none;
      pointer-events: none;
      opacity: 0.8;
      animation: float 6s infinite ease-in-out;
    }

    @keyframes float {
      0% { transform: translateY(100vh) scale(0.5); opacity: 0; }
      10% { opacity: 1; }
      100% { transform: translateY(-10vh) scale(1); opacity: 0; }
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 0;
    }

    .logo {
      position: fixed;
      bottom: 10px;
      right: 15px;
      font-size: 1rem;
      color: #fff;
      opacity: 0.8;
      letter-spacing: 1px;
      font-weight: 500;
      z-index: 10;
    }

    .logo span {
      color: #ffcc70;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <canvas id="fireworks"></canvas>
  <div class="container">
    <h1>🎑 Trung thu vui vẻ 🎑</h1>
    <p>Chúc chị sớm có người yêu 💖</p>
    <audio autoplay loop>
      <source src="https://cdn.pixabay.com/download/audio/2023/02/09/audio_792af0e99d.mp3?filename=mid-autumn-festival-remix-144357.mp3" type="audio/mpeg">
    </audio>
  </div>

  <div class="logo">✨ <span>Quang Toàn Studio</span> ✨</div>

  <script>
    // Hiệu ứng tim và đèn lồng
    const body = document.body;
    function createFloating(emoji, sizeMin=20, sizeMax=50, durationMin=6, durationMax=10) {
      const e = document.createElement("div");
      e.className = "emoji";
      e.textContent = emoji;
      e.style.left = Math.random() * 100 + "vw";
      e.style.fontSize = Math.random() * (sizeMax - sizeMin) + sizeMin + "px";
      e.style.animationDuration = Math.random() * (durationMax - durationMin) + durationMin + "s";
      body.appendChild(e);
      setTimeout(() => e.remove(), 12000);
    }
    setInterval(() => createFloating("💖"), 600);
    setInterval(() => createFloating("🏮"), 1200);

    // Pháo hoa 🎆
    const canvas = document.getElementById('fireworks');
    const ctx = canvas.getContext('2d');
    let w, h;
    function resize() { w = canvas.width = window.innerWidth; h = canvas.height = window.innerHeight; }
    window.addEventListener('resize', resize);
    resize();

    const fireworks = [];
    const particles = [];
    function random(min, max) { return Math.random() * (max - min) + min; }

    function createFirework() {
      fireworks.push({
        x: random(w*0.2, w*0.8),
        y: h,
        targetY: random(h*0.2, h*0.5),
        speed: random(3, 6),
        hue: random(0, 360),
        exploded: false
      });
    }

    function loop() {
      ctx.fillStyle = "rgba(0,0,0,0.15)";
      ctx.fillRect(0, 0, w, h);

      for (let i = fireworks.length - 1; i >= 0; i--) {
        const f = fireworks[i];
        ctx.beginPath();
        ctx.arc(f.x, f.y, 2, 0, Math.PI * 2);
        ctx.fillStyle = `hsl(${f.hue}, 100%, 70%)`;
        ctx.fill();
        f.y -= f.speed;
        if (f.y <= f.targetY && !f.exploded) {
          f.exploded = true;
          for (let j = 0; j < 50; j++) {
            particles.push({
              x: f.x, y: f.y,
              vx: random(-4, 4),
              vy: random(-4, 4),
              life: random(50, 100),
              hue: f.hue + random(-20, 20)
            });
          }
          fireworks.splice(i, 1);
        }
      }

      for (let i = particles.length - 1; i >= 0; i--) {
        const p = particles[i];
        p.x += p.vx;
        p.y += p.vy;
        p.vy += 0.05;
        p.life--;
        ctx.beginPath();
        ctx.arc(p.x, p.y, 2, 0, Math.PI * 2);
        ctx.fillStyle = `hsla(${p.hue}, 100%, 60%, ${p.life / 100})`;
        ctx.fill();
        if (p.life <= 0) particles.splice(i, 1);
      }

      requestAnimationFrame(loop);
    }

    setInterval(createFirework, 800);
    loop();
  </script>
</body>
</html>
