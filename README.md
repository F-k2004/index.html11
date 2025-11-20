<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8" />
  <title>🕰️ ساعت آنالوگ با صدای تیک‌تاک</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: radial-gradient(circle at center, #111, #000);
      font-family: sans-serif;
      user-select: none;
      color: white;
    }

    .clock {
      width: 300px;
      height: 300px;
      border: 10px solid rgba(255,255,255,0.15);
      border-radius: 50%;
      position: relative;
      background: rgba(255,255,255,0.05);
      box-shadow: 0 0 25px rgba(255,255,255,0.15);
      backdrop-filter: blur(6px);
    }

    .center {
      width: 14px;
      height: 14px;
      background: white;
      border-radius: 50%;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 10;
    }

    .hand {
      position: absolute;
      width: 50%;
      height: 2px;
      background: white;
      top: 50%;
      transform-origin: 100%;
      transform: rotate(90deg);
      transition: all 0.05s linear;
    }

    .hour { height: 4px; width: 35%; background: #f0f0f0; }
    .minute { height: 3px; width: 45%; background: #ddd; }
    .second { height: 2px; width: 48%; background: #ff3b3b; }

    .numbers {
      position: absolute;
      width: 100%;
      height: 100%;
      font-size: 24px;
    }

    .numbers div {
      position: absolute;
      transform: translate(-50%, -50%);
    }
  </style>
</head>

<body>

  <div class="clock">
    <div class="numbers">
      <div style="top: 20px; left: 50%;">12</div>
      <div style="top: 50%; left: 280px;">3</div>
      <div style="top: 280px; left: 50%;">6</div>
      <div style="top: 50%; left: 20px;">9</div>
    </div>

    <div class="hand hour" id="hour"></div>
    <div class="hand minute" id="minute"></div>
    <div class="hand second" id="second"></div>

    <div class="center"></div>
  </div>

  <script>
    function updateClock() {
      const now = new Date();

      const seconds = now.getSeconds();
      const minutes = now.getMinutes();
      const hours = now.getHours();

      // چرخش عقربه‌ها
      document.getElementById("second").style.transform = `rotate(${seconds * 6}deg)`;
      document.getElementById("minute").style.transform = `rotate(${minutes * 6 + seconds * 0.1}deg)`;
      document.getElementById("hour").style.transform = `rotate(${hours * 30 + minutes * 0.5}deg)`;
    }

    // اجرای ساعت
    setInterval(() => {
      updateClock();
      playTick();
    }, 1000);

    updateClock();

    // -------------------------------
    // 🎵 سیستم تولید صدای "تیک تاک"
    // -------------------------------
    const audioContext = new (window.AudioContext || window.webkitAudioContext)();

    function playTick() {
      const osc = audioContext.createOscillator();
      const gain = audioContext.createGain();

      osc.frequency.value = 800;   // فرکانس صدای تیک
      gain.gain.value = 0.15;      // شدت صدا

      osc.connect(gain);
      gain.connect(audioContext.destination);

      osc.start();
      osc.stop(audioContext.currentTime + 0.05); // مدت بسیار کوتاه (تقریباً تیک واقعی)
    }

    // لازم است کاربر یک بار روی صفحه کلیک کند تا صدا فعال شود
    document.body.addEventListener("click", () => {
      audioContext.resume();
    });
  </script>

</body>
</html>
