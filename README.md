<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>A Secret Letter For You 💌</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Poppins', 'Segoe UI', sans-serif;
    }

    body {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      background: linear-gradient(-45deg, #2e1065, #581c87, #7e22ce, #9333ea, #a855f7, #6b21a8);
      background-size: 400% 400%;
      animation: purpleGradient 12s ease infinite;
      overflow: hidden;
      padding: 20px;
    }

    @keyframes purpleGradient {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }

    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 30px;
      perspective: 1000px;
    }

    .instruction {
      color: #f3e8ff;
      font-size: 1.4rem;
      font-weight: 600;
      text-shadow: 0 0 12px rgba(236, 72, 153, 0.8);
      animation: bounce 2s infinite;
      text-align: center;
    }

    @keyframes bounce {
      0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
      40% { transform: translateY(-10px); }
      60% { transform: translateY(-5px); }
    }

    /* Envelope Styling */
    .envelope-wrapper {
      position: relative;
      width: 320px;
      height: 220px;
      background: #7e22ce;
      border-bottom-left-radius: 12px;
      border-bottom-right-radius: 12px;
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.4);
      cursor: pointer;
    }

    .letter {
      position: absolute;
      bottom: 10px;
      left: 15px;
      width: 290px;
      height: 190px;
      background: #ffffff;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
      transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
      z-index: 2;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
    }

    .letter h3 {
      color: #9333ea;
      font-size: 1.2rem;
      margin-bottom: 8px;
    }

    .letter p {
      color: #64748b;
      font-size: 0.9rem;
    }

    /* Envelope Flaps */
    .pocket {
      position: absolute;
      width: 0;
      height: 0;
      border-left: 160px solid #a855f7;
      border-right: 160px solid #a855f7;
      border-bottom: 110px solid #9333ea;
      border-top: 110px solid transparent;
      border-bottom-left-radius: 12px;
      border-bottom-right-radius: 12px;
      top: 0;
      left: 0;
      z-index: 3;
    }

    .lid {
      position: absolute;
      width: 0;
      height: 0;
      border-left: 160px solid transparent;
      border-right: 160px solid transparent;
      border-top: 120px solid #7e22ce;
      border-bottom: 0 solid transparent;
      top: 0;
      left: 0;
      transform-origin: top;
      transition: transform 0.6s ease-in-out;
      z-index: 4;
    }

    .wax-seal {
      position: absolute;
      top: 90px;
      left: 135px;
      width: 50px;
      height: 50px;
      background: radial-gradient(circle, #ec4899, #be185d);
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      color: #fff;
      font-size: 1.5rem;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
      z-index: 5;
      transition: opacity 0.4s ease, transform 0.4s ease;
    }

    /* Open State Animations */
    .envelope-wrapper.open .lid {
      transform: rotateX(180deg);
      z-index: 1;
    }

    .envelope-wrapper.open .wax-seal {
      opacity: 0;
      transform: scale(0.5);
    }

    .envelope-wrapper.open .letter {
      transform: translateY(-130px);
      z-index: 3;
    }
  </style>
</head>
<body>

  <div class="container">
    <div class="instruction">Tap the envelope to open 💌</div>

    <div class="envelope-wrapper" id="envelope">
      <div class="lid"></div>
      <div class="wax-seal">💖</div>
      <div class="pocket"></div>
      <div class="letter">
        <h3>For My Beloved Wife</h3>
        <p>Opening your special letter...</p>
      </div>
    </div>
  </div>

  <script>
    const envelope = document.getElementById('envelope');

    envelope.addEventListener('click', () => {
      if (!envelope.classList.contains('open')) {
        // Trigger envelope unfolding animation
        envelope.classList.add('open');

        // Play ambient chime sound using Web Audio API
        playChime();

        // Redirect or load main content after 1.8 seconds
        setTimeout(() => {
          // Replace 'letter.html' with the filename of your main page, or put custom logic here
          window.location.href = "love_letter.html";
        }, 1800);
      }
    });

    function playChime() {
      try {
        const AudioCtx = window.AudioContext || window.webkitAudioContext;
        const ctx = new AudioCtx();
        const notes = [523.25, 659.25, 783.99, 1046.50]; // C5, E5, G5, C6
        
        notes.forEach((freq, idx) => {
          const osc = ctx.createOscillator();
          const gain = ctx.createGain();
          osc.type = 'sine';
          osc.frequency.value = freq;
          
          gain.gain.setValueAtTime(0, ctx.currentTime + idx * 0.15);
          gain.gain.linearRampToValueAtTime(0.15, ctx.currentTime + idx * 0.15 + 0.05);
          gain.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime + idx * 0.15 + 0.8);
          
          osc.connect(gain);
          gain.connect(ctx.destination);
          
          osc.start(ctx.currentTime + idx * 0.15);
          osc.stop(ctx.currentTime + idx * 0.15 + 0.8);
        });
      } catch (e) {
        console.log("Audio play blocked");
      }
    }
  </script>
</body>
</html>
