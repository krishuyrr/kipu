Index.html
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Will u be my best friend? 💖 | Krish &amp; Kipu</title>
  <!-- Tailwind CSS for responsive glassmorphic layout -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Three.js for interactive 3D floating extruded hearts and stars -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <!-- Canvas Confetti for YES celebration -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;600;700;800&family=Space+Grotesk:wght@500;700&display=swap" rel="stylesheet" />
  <style>
    * {
      box-sizing: border-box;
      -webkit-tap-highlight-color: transparent;
    }

    html, body {
      margin: 0;
      padding: 0;
      min-height: 100%;
      font-family: 'Outfit', sans-serif;
      background: radial-gradient(circle at top, #1f0a38 0%, #0d051a 55%, #06020d 100%);
      color: #ffffff;
      overflow-x: hidden;
    }

    /* Fullscreen WebGL canvas behind the glassmorphic UI */
    #bg-canvas {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      z-index: 0;
      pointer-events: none;
    }

    .app-shell {
      position: relative;
      z-index: 10;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      padding: 18px 14px 28px;
    }

    .glass-card {
      background: rgba(24, 11, 44, 0.72);
      backdrop-filter: blur(18px);
      -webkit-backdrop-filter: blur(18px);
      border: 1px solid rgba(255, 130, 210, 0.24);
      box-shadow: 0 18px 45px rgba(0, 0, 0, 0.45), inset 0 1px 0 rgba(255, 255, 255, 0.12);
    }

    /* Floating 3D badges for Krish and Kipu */
    .badge-3d {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      padding: 10px 20px;
      border-radius: 999px;
      background: linear-gradient(135deg, rgba(255, 79, 173, 0.28), rgba(139, 92, 246, 0.34));
      border: 1px solid rgba(255, 175, 226, 0.48);
      box-shadow: 0 8px 0 rgba(76, 20, 120, 0.7), 0 14px 28px rgba(255, 64, 160, 0.35);
      transform-style: preserve-3d;
      animation: floatBadge 3.4s ease-in-out infinite;
      font-family: 'Space Grotesk', sans-serif;
      letter-spacing: 0.03em;
    }

    .badge-3d-footer {
      animation: floatBadgeReverse 3.8s ease-in-out infinite;
    }

    @keyframes floatBadge {
      0%, 100% { transform: translateY(0px) rotateX(6deg) rotateY(-4deg); }
      50% { transform: translateY(-6px) rotateX(-4deg) rotateY(5deg); }
    }

    @keyframes floatBadgeReverse {
      0%, 100% { transform: translateY(0px) rotateX(-5deg) rotateY(4deg); }
      50% { transform: translateY(-5px) rotateX(5deg) rotateY(-5deg); }
    }

    .neon-title {
      background: linear-gradient(90deg, #ff79c6, #f8a5ff, #9d7bff, #ff79c6);
      background-size: 220% auto;
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      animation: shineText 4.5s linear infinite;
    }

    @keyframes shineText {
      to { background-position: 220% center; }
    }

    .vibe-option {
      width: 100%;
      text-align: left;
      padding: 14px 16px;
      border-radius: 18px;
      background: rgba(255, 255, 255, 0.04);
      border: 1px solid rgba(255, 255, 255, 0.12);
      transition: transform 0.2s ease, border-color 0.2s ease, background 0.2s ease, box-shadow 0.2s ease;
    }

    .vibe-option:hover {
      border-color: rgba(255, 121, 198, 0.55);
      background: rgba(255, 121, 198, 0.1);
    }

    .vibe-option.active {
      border-color: #ff6ec7;
      background: linear-gradient(135deg, rgba(255, 92, 184, 0.24), rgba(139, 92, 246, 0.24));
      box-shadow: 0 0 22px rgba(255, 110, 199, 0.28);
      transform: scale(1.01);
    }

    /* Arena where the evasive NO button dodges without overlapping YES */
    .choice-arena {
      position: relative;
      min-height: 136px;
      width: 100%;
      border-radius: 22px;
      background: rgba(12, 5, 24, 0.55);
      border: 1px dashed rgba(255, 150, 215, 0.25);
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      padding: 16px;
    }

    #yes-btn {
      position: relative;
      z-index: 20;
      padding: 14px 30px;
      border-radius: 999px;
      font-weight: 800;
      font-size: 1.05rem;
      background: linear-gradient(135deg, #ff4da6, #9b5de5);
      box-shadow: 0 6px 0 #5b188a, 0 12px 26px rgba(255, 77, 166, 0.45);
      transition: transform 0.16s ease, box-shadow 0.16s ease;
      flex-shrink: 0;
    }

    #yes-btn:active {
      transform: translateY(3px);
      box-shadow: 0 3px 0 #5b188a, 0 8px 18px rgba(255, 77, 166, 0.4);
    }

    #no-btn {
      position: relative;
      z-index: 20;
      padding: 13px 24px;
      border-radius: 999px;
      font-weight: 700;
      font-size: 0.98rem;
      background: rgba(255, 255, 255, 0.09);
      border: 1px solid rgba(255, 255, 255, 0.22);
      color: #f3d9ff;
      transition: left 0.22s cubic-bezier(0.22, 1, 0.36, 1), top 0.22s cubic-bezier(0.22, 1, 0.36, 1), transform 0.18s ease;
      flex-shrink: 0;
      user-select: none;
      touch-action: none;
    }

    .floating-heart {
      position: fixed;
      pointer-events: none;
      z-index: 40;
      font-size: 24px;
      animation: popUpHeart 1.25s ease-out forwards;
    }

    @keyframes popUpHeart {
      0% { opacity: 1; transform: translate(-50%, 0) scale(0.6) rotate(-10deg); }
      100% { opacity: 0; transform: translate(-50%, -115px) scale(1.45) rotate(18deg); }
    }
  </style>
</head>
<body>
  <canvas id="bg-canvas"></canvas>

  <!-- Hidden YouTube IFrame host for "JMSN - Love Me" -->
  <div id="yt-player-host" class="sr-only" aria-hidden="true">
    <div id="yt-love-me-player"></div>
  </div>

  <div class="app-shell max-w-md mx-auto w-full gap-5">
    <!-- Top Floating 3D Badge -->
    <header class="text-center pt-1">
      <div class="badge-3d text-sm sm:text-base font-bold text-pink-100">
        <span>✨</span>
        <span>Made by krish for kipu</span>
        <span>💖</span>
      </div>
    </header>

    <!-- Main Interactive Sections -->
    <main class="flex flex-col gap-4 my-auto">
      <!-- Hero Card with Headline & Music Controls -->
      <section class="glass-card rounded-3xl p-6 text-center">
        <div class="flex items-center justify-between gap-2 mb-4">
          <span class="text-xs uppercase tracking-widest px-3 py-1 rounded-full bg-pink-500/20 text-pink-200 border border-pink-400/30 font-semibold">
            3D Pookie Edition
          </span>
          <button
            id="music-btn"
            type="button"
            class="text-xs px-3.5 py-1.5 rounded-full bg-white/10 hover:bg-white/15 border border-white/20 text-purple-100 font-semibold transition"
          >
            🎵 Love Me: Tap to Play
          </button>
        </div>

        <h1 class="text-3xl sm:text-4xl font-extrabold neon-title leading-tight mb-2">
          Will u be my best friend? 💖
        </h1>
        <p class="text-sm text-purple-200/85 leading-relaxed mb-2.5">
          Part-time bestie, full-time main character energy, and zero boring days guaranteed ✨
        </p>

        <div class="flex flex-col items-center gap-1.5">
          <p id="song-status" class="text-[11px] text-pink-200/85">
            🎶 Now Cueing: JMSN — Love Me (autoplay attempt + tap unlock)
          </p>
          <a
            id="yt-direct-link"
            href="https://youtu.be/60rLYaz9Q1w?si=HdpifTdA6xyQj5Um"
            target="_blank"
            rel="noopener noreferrer"
            class="inline-flex items-center gap-1.5 text-[11px] font-semibold text-pink-300 hover:text-pink-200 underline underline-offset-4 transition"
          >
            <span>▶️ Watch "Love Me" on YouTube</span>
          </a>
        </div>
      </section>

      <!-- Interactive Rizz Card -->
      <section class="glass-card rounded-3xl p-5">
        <div class="flex items-center justify-between mb-2">
          <span class="text-xs font-bold uppercase tracking-wider text-pink-300">
            💌 Daily Rizz Drop #<span id="rizz-index">1</span>
          </span>
          <span class="text-xs text-purple-300/75">Tap for 3D hearts</span>
        </div>

        <p id="rizz-text" class="text-base sm:text-lg font-semibold text-white min-h-[56px] flex items-center leading-snug mb-4">
          "Are you Wi-Fi? Because I feel a strong connection every time you pop up on my screen 📶💖"
        </p>

        <button
          id="next-rizz-btn"
          type="button"
          class="w-full py-3 px-4 rounded-2xl font-bold text-sm bg-gradient-to-r from-pink-500/25 via-purple-500/25 to-indigo-500/25 hover:from-pink-500/35 hover:to-purple-500/35 border border-pink-400/40 text-pink-100 transition active:scale-[0.99]"
        >
          💘 Next Rizz &amp; 3D Heart Burst
        </button>
      </section>

      <!-- Quick Vibe Check Quiz -->
      <section class="glass-card rounded-3xl p-5">
        <div class="mb-3">
          <h2 class="text-lg font-extrabold text-white flex items-center gap-2">
            <span>🕵️‍♂️</span>
            <span>Quick Vibe Check</span>
          </h2>
          <p class="text-xs text-purple-200/80">
            Pick your current chat vibe before hitting YES...
          </p>
        </div>

        <div class="flex flex-col gap-2.5" id="vibe-options">
          <button
            type="button"
            class="vibe-option"
            data-label="Single &amp; 100% Free ✨"
            data-reply="Best news ever. I'm officially claiming the #1 spot in your notifications 💖"
          >
            <div class="flex items-center justify-between">
              <span class="font-bold text-sm text-white">Single &amp; 100% Free ✨</span>
              <span class="text-xs px-2 py-0.5 rounded-full bg-emerald-400/20 text-emerald-200">Open</span>
            </div>
            <p class="text-xs text-purple-200/70 mt-0.5">No serious chat drama right now!</p>
          </button>

          <button
            type="button"
            class="vibe-option"
            data-label="Just friendly chats, nothing serious 🤫"
            data-reply="Sounds like background noise. I'm here to upgrade you to permanent main-character bestie energy 🚀"
          >
            <div class="flex items-center justify-between">
              <span class="font-bold text-sm text-white">Just friendly chats, nothing serious 🤫</span>
              <span class="text-xs px-2 py-0.5 rounded-full bg-amber-400/20 text-amber-200">Casual</span>
            </div>
            <p class="text-xs text-purple-200/70 mt-0.5">Nobody special holding the crown yet.</p>
          </button>

          <button
            type="button"
            class="vibe-option"
            data-label="Secretly hoping it was you all along 👑"
            data-reply="Plot twist accepted with full VIP privileges. Top-tier bestie &amp; rizz mode unlocked 👑💖"
          >
            <div class="flex items-center justify-between">
              <span class="font-bold text-sm text-white">Secretly hoping it was you all along 👑</span>
              <span class="text-xs px-2 py-0.5 rounded-full bg-pink-400/25 text-pink-200">VIP</span>
            </div>
            <p class="text-xs text-purple-200/70 mt-0.5">Top priority unlocked!</p>
          </button>
        </div>

        <div
          id="vibe-response"
          class="hidden mt-3.5 p-3.5 rounded-2xl bg-pink-500/15 border border-pink-400/35 text-xs sm:text-sm text-pink-100 leading-relaxed"
        ></div>
      </section>

      <!-- YES / Evasive NO Choice Arena -->
      <section class="glass-card rounded-3xl p-5 text-center">
        <h2 class="text-xl font-extrabold text-white mb-1">So... Is It A YES?</h2>
        <p class="text-xs text-purple-200/80 mb-4">
          Careful — the "NO" button has 3D dodge reflexes! 🙈
        </p>

        <div id="choice-arena" class="choice-arena gap-4">
          <button id="yes-btn" type="button">YES! 💖</button>
          <button id="no-btn" type="button">NO 🙈</button>
        </div>

        <p id="dodge-counter" class="text-[11px] text-pink-200/75 mt-2.5 min-h-[16px]"></p>
      </section>
    </main>

    <!-- Bottom Floating 3D Badge -->
    <footer class="text-center pt-1">
      <div class="badge-3d badge-3d-footer text-sm sm:text-base font-bold text-pink-100">
        <span>💜</span>
        <span>Made by krish for kipu</span>
        <span>✨</span>
      </div>
    </footer>
  </div>

  <!-- Celebration & WhatsApp Modal -->
  <div
    id="yes-modal"
    class="fixed inset-0 z-50 hidden items-center justify-center p-4 bg-black/75 backdrop-blur-md"
  >
    <div class="glass-card max-w-sm w-full rounded-3xl p-6 text-center relative border-pink-400/50">
      <button
        id="close-modal-btn"
        type="button"
        aria-label="Close"
        class="absolute top-4 right-4 w-8 h-8 rounded-full bg-white/10 hover:bg-white/20 text-white text-sm font-bold flex items-center justify-center"
      >
        ✕
      </button>

      <div class="text-4xl mb-2">🥳💖✨</div>
      <h3 class="text-2xl font-extrabold neon-title mb-1.5">Bestie Confirmed!</h3>
      <p class="text-sm text-purple-100/90 mb-4">
        Send your official reply straight to Krish on WhatsApp!
      </p>

      <div class="text-left rounded-2xl bg-black/40 border border-white/15 p-3.5 mb-4">
        <div class="text-[11px] uppercase tracking-wider text-pink-300 font-bold mb-1">
          Message Preview (+91 7986168908)
        </div>
        <pre id="wa-preview-text" class="text-xs text-purple-100 whitespace-pre-wrap font-sans leading-relaxed"></pre>
      </div>

      <div class="flex flex-col gap-2.5">
        <a
          id="whatsapp-link"
          href="https://wa.me/917986168908"
          target="_blank"
          rel="noopener noreferrer"
          class="w-full py-3.5 px-4 rounded-2xl font-extrabold text-sm bg-gradient-to-r from-emerald-500 to-teal-500 hover:from-emerald-400 hover:to-teal-400 text-white shadow-lg flex items-center justify-center gap-2 transition"
        >
          <span>💬</span>
          <span>Send WhatsApp Message</span>
        </a>
        <button
          id="copy-msg-btn"
          type="button"
          class="w-full py-2.5 px-4 rounded-2xl font-semibold text-xs bg-white/10 hover:bg-white/15 border border-white/15 text-purple-100 transition"
        >
          📋 Copy Message Text
        </button>
      </div>
    </div>
  </div>

  <script>
    // 1. Rizz Lines State
    const rizzLines = [
      '"Are you Wi-Fi? Because I feel a strong connection every time you pop up on my screen 📶💖"',
      '"If being cute were a full-time job, Kipu would already have employee of the year ✨"',
      '"My favorite notification isn\'t even a ringtone — it\'s seeing your name pop up 📱💜"',
      '"I tried acting cool today, and then I remembered how fun it would be if you said YES 😉"',
      '"Best friends are supposed to share snacks, laughs, and elite rizz — deal? 🍓✨"'
    ];

    let rizzIdx = 0;
    let selectedVibe = "Not picked yet (went straight for YES!)";

    const rizzTextEl = document.getElementById("rizz-text");
    const rizzIndexEl = document.getElementById("rizz-index");
    const nextRizzBtn = document.getElementById("next-rizz-btn");

    // 2. Three.js 3D Scene: Floating Extruded Hearts + Starfield + Parallax
    let scene, camera, renderer, hearts3D = [], stars3D;
    let pointerX = 0, pointerY = 0;

    function initThree() {
      const canvas = document.getElementById("bg-canvas");
      if (!window.THREE || !canvas) return;

      try {
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 100);
        camera.position.z = 14;

        renderer = new THREE.WebGLRenderer({ canvas, alpha: true, antialias: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio || 1, 2));

        const ambientLight = new THREE.AmbientLight(0xffb3ec, 1.3);
        scene.add(ambientLight);

        const pointLight1 = new THREE.PointLight(0xff4da6, 2.5, 45);
        pointLight1.position.set(6, 6, 10);
        scene.add(pointLight1);

        const pointLight2 = new THREE.PointLight(0x8b5cf6, 2.2, 45);
        pointLight2.position.set(-7, -5, 8);
        scene.add(pointLight2);

        // Extruded 3D Heart Geometry
        const heartShape = new THREE.Shape();
        heartShape.moveTo(0, 0.45);
        heartShape.bezierCurveTo(0, 0.8, -0.55, 1.05, -0.95, 0.65);
        heartShape.bezierCurveTo(-1.35, 0.25, -1.15, -0.4, -0.65, -0.82);
        heartShape.lineTo(0, -1.35);
        heartShape.lineTo(0.65, -0.82);
        heartShape.bezierCurveTo(1.15, -0.4, 1.35, 0.25, 0.95, 0.65);
        heartShape.bezierCurveTo(0.55, 1.05, 0, 0.8, 0, 0.45);

        const extrudeGeo = new THREE.ExtrudeGeometry(heartShape, {
          depth: 0.35,
          bevelEnabled: true,
          bevelSegments: 3,
          steps: 1,
          bevelSize: 0.08,
          bevelThickness: 0.08
        });

        const colors = [0xff5ec4, 0xc084fc, 0xf472b6, 0xa78bfa];

        for (let i = 0; i < 14; i++) {
          const mat = new THREE.MeshStandardMaterial({
            color: colors[i % colors.length],
            roughness: 0.28,
            metalness: 0.25,
            emissive: colors[i % colors.length],
            emissiveIntensity: 0.18
          });

          const mesh = new THREE.Mesh(extrudeGeo, mat);
          const scale = 0.42 + Math.random() * 0.48;
          mesh.scale.set(scale, scale, scale);
          mesh.position.set(
            (Math.random() - 0.5) * 14,
            (Math.random() - 0.5) * 14,
            (Math.random() - 0.5) * 8 - 2
          );
          mesh.rotation.set(
            (Math.random() - 0.5) * 0.6,
            (Math.random() - 0.5) * Math.PI,
            Math.PI
          );
          mesh.userData = {
            speedY: 0.004 + Math.random() * 0.007,
            rotSpeed: 0.008 + Math.random() * 0.012,
            phase: Math.random() * Math.PI * 2,
            baseX: mesh.position.x
          };
          scene.add(mesh);
          hearts3D.push(mesh);
        }

        // Starfield Points
        const starCount = 260;
        const starPositions = new Float32Array(starCount * 3);
        for (let i = 0; i < starCount * 3; i += 3) {
          starPositions[i] = (Math.random() - 0.5) * 34;
          starPositions[i + 1] = (Math.random() - 0.5) * 34;
          starPositions[i + 2] = (Math.random() - 0.5) * 26 - 4;
        }
        const starGeo = new THREE.BufferGeometry();
        starGeo.setAttribute("position", new THREE.BufferAttribute(starPositions, 3));
        const starMat = new THREE.PointsMaterial({
          color: 0xfce7f3,
          size: 0.11,
          transparent: true,
          opacity: 0.78
        });
        stars3D = new THREE.Points(starGeo, starMat);
        scene.add(stars3D);

        window.addEventListener("resize", () => {
          if (!renderer || !camera) return;
          camera.aspect = window.innerWidth / window.innerHeight;
          camera.updateProjectionMatrix();
          renderer.setSize(window.innerWidth, window.innerHeight);
        });

        const updatePointer = (x, y) => {
          pointerX = (x / window.innerWidth) * 2 - 1;
          pointerY = -((y / window.innerHeight) * 2 - 1);
        };

        window.addEventListener("pointermove", (e) => updatePointer(e.clientX, e.clientY), { passive: true });
        window.addEventListener("touchmove", (e) => {
          if (e.touches && e.touches[0]) {
            updatePointer(e.touches[0].clientX, e.touches[0].clientY);
          }
        }, { passive: true });

        animateThree();
      } catch (_) {}
    }

    function burst3DHearts() {
      hearts3D.forEach((h) => {
        h.position.z = Math.min(4, h.position.z + 1.4);
        h.rotation.y += 0.5;
      });
    }

    function animateThree() {
      requestAnimationFrame(animateThree);
      if (!renderer || !scene || !camera) return;

      const t = performance.now() * 0.001;
      camera.position.x += (pointerX * 1.4 - camera.position.x) * 0.04;
      camera.position.y += (pointerY * 1.1 - camera.position.y) * 0.04;
      camera.lookAt(0, 0, 0);

      hearts3D.forEach((h) => {
        h.position.y += h.userData.speedY;
        h.position.x = h.userData.baseX + Math.sin(t + h.userData.phase) * 0.45;
        h.rotation.y += h.userData.rotSpeed;
        if (h.position.y > 9.5) {
          h.position.y = -9.5;
          h.userData.baseX = (Math.random() - 0.5) * 13;
        }
      });

      if (stars3D) {
        stars3D.rotation.y = t * 0.02;
      }

      renderer.render(scene, camera);
    }

    initThree();

    // 3. Floating Emoji Heart Burst & Rizz Cycling
    function spawnFloatingHeart(x, y) {
      const emojis = ["💖", "✨", "💜", "💘", "🌸"];
      const el = document.createElement("div");
      el.className = "floating-heart";
      el.textContent = emojis[Math.floor(Math.random() * emojis.length)];
      el.style.left = `${x || window.innerWidth / 2}px`;
      el.style.top = `${y || window.innerHeight / 2}px`;
      document.body.appendChild(el);
      setTimeout(() => el.remove(), 1300);
    }

    nextRizzBtn.addEventListener("click", (e) => {
      rizzIdx = (rizzIdx + 1) % rizzLines.length;
      rizzTextEl.textContent = rizzLines[rizzIdx];
      rizzIndexEl.textContent = String(rizzIdx + 1);
      burst3DHearts();
      spawnFloatingHeart(e.clientX, e.clientY);
    });

    // 4. Quick Vibe Check Selection & Reply
    const vibeBtns = document.querySelectorAll(".vibe-option");
    const vibeResponseBox = document.getElementById("vibe-response");

    vibeBtns.forEach((btn) => {
      btn.addEventListener("click", (e) => {
        vibeBtns.forEach((b) => b.classList.remove("active"));
        btn.classList.add("active");
        selectedVibe = btn.dataset.label || "Selected";
        vibeResponseBox.innerHTML = `<span class="font-bold text-pink-300">Krish's Reply:</span> ${btn.dataset.reply}`;
        vibeResponseBox.classList.remove("hidden");
        burst3DHearts();
        spawnFloatingHeart(e.clientX, e.clientY);
      });
    });

    // 5. Evasive NO Button (Dodges inside choice-arena without overlapping YES)
    const arena = document.getElementById("choice-arena");
    const noBtn = document.getElementById("no-btn");
    const yesBtn = document.getElementById("yes-btn");
    const dodgeCounter = document.getElementById("dodge-counter");
    let dodgeCount = 0;

    const dodgePhrases = [
      "Nice try! That button has speedrun reflexes 😂",
      "NO is currently out of stock for Kipu 🙈",
      "Even the 3D hearts said press YES 💖",
      "Too slow! Bestie mode is inevitable ✨"
    ];

    function dodgeNoButton(e) {
      if (e && e.cancelable) e.preventDefault();
      dodgeCount += 1;

      const arenaRect = arena.getBoundingClientRect();
      const yesRect = yesBtn.getBoundingClientRect();
      const btnW = noBtn.offsetWidth || 96;
      const btnH = noBtn.offsetHeight || 46;

      noBtn.style.position = "absolute";

      const maxX = Math.max(10, arenaRect.width - btnW - 10);
      const maxY = Math.max(10, arenaRect.height - btnH - 10);

      let nextX = 12;
      let nextY = 12;

      for (let attempt = 0; attempt < 20; attempt++) {
        const candX = 10 + Math.random() * maxX;
        const candY = 10 + Math.random() * maxY;

        const candLeft = arenaRect.left + candX;
        const candRight = candLeft + btnW;
        const candTop = arenaRect.top + candY;
        const candBottom = candTop + btnH;

        const overlapsYes = !(
          candRight + 14 < yesRect.left ||
          candLeft - 14 > yesRect.right ||
          candBottom + 14 < yesRect.top ||
          candTop - 14 > yesRect.bottom
        );

        if (!overlapsYes) {
          nextX = candX;
          nextY = candY;
          break;
        }
      }

      noBtn.style.left = `${nextX}px`;
      noBtn.style.top = `${nextY}px`;
      dodgeCounter.textContent = `${dodgePhrases[dodgeCount % dodgePhrases.length]} (${dodgeCount})`;
    }

    noBtn.addEventListener("pointerenter", dodgeNoButton);
    noBtn.addEventListener("touchstart", dodgeNoButton, { passive: false });
    noBtn.addEventListener("click", dodgeNoButton);

    // 6. YES Celebration + WhatsApp Deep Link (+917986168908) + Copy
    const modal = document.getElementById("yes-modal");
    const closeModalBtn = document.getElementById("close-modal-btn");
    const waPreviewText = document.getElementById("wa-preview-text");
    const whatsappLink = document.getElementById("whatsapp-link");
    const copyMsgBtn = document.getElementById("copy-msg-btn");

    function buildWhatsAppMessage() {
      return [
        "Hey Krish! 💖",
        "Answer: YES, I'll be your best friend! ✨",
        `🕵️‍♂️ Vibe Check: ${selectedVibe}`
      ].join("\n");
    }

    yesBtn.addEventListener("click", () => {
      const msg = buildWhatsAppMessage();
      waPreviewText.textContent = msg;
      whatsappLink.href = `https://wa.me/917986168908?text=${encodeURIComponent(msg)}`;

      modal.classList.remove("hidden");
      modal.classList.add("flex");

      burst3DHearts();
      startLoveMeSong();

      if (window.confetti) {
        confetti({
          particleCount: 95,
          spread: 75,
          origin: { y: 0.65 }
        });
      }
    });

    closeModalBtn.addEventListener("click", () => {
      modal.classList.add("hidden");
      modal.classList.remove("flex");
    });

    modal.addEventListener("click", (e) => {
      if (e.target === modal) {
        modal.classList.add("hidden");
        modal.classList.remove("flex");
      }
    });

    copyMsgBtn.addEventListener("click", async () => {
      const text = buildWhatsAppMessage();
      try {
        await navigator.clipboard.writeText(text);
        copyMsgBtn.textContent = "✅ Copied!";
        setTimeout(() => { copyMsgBtn.textContent = "📋 Copy Message Text"; }, 1800);
      } catch (_) {
        const tempInput = document.createElement("textarea");
        tempInput.value = text;
        document.body.appendChild(tempInput);
        tempInput.select();
        try {
          document.execCommand("copy");
          copyMsgBtn.textContent = "✅ Copied!";
        } catch (err) {
          copyMsgBtn.textContent = "📋 Ready to copy above";
        }
        tempInput.remove();
        setTimeout(() => { copyMsgBtn.textContent = "📋 Copy Message Text"; }, 1800);
      }
    });

    // 7. YouTube IFrame API Integration for "JMSN - Love Me" (60rLYaz9Q1w)
    const LOVE_ME_VIDEO_ID = "60rLYaz9Q1w";
    const LOVE_ME_YT_URL = "https://youtu.be/60rLYaz9Q1w?si=HdpifTdA6xyQj5Um";
    const musicBtn = document.getElementById("music-btn");
    const songStatus = document.getElementById("song-status");
    const ytDirectLink = document.getElementById("yt-direct-link");

    if (ytDirectLink) {
      ytDirectLink.href = LOVE_ME_YT_URL;
    }

    let ytPlayer = null;
    let ytReady = false;
    let isLoveMePlaying = false;
    let userUnlockedAudio = false;

    function updateSongUI(playing, labelText, statusText) {
      isLoveMePlaying = playing;
      musicBtn.textContent = labelText;
      if (statusText && songStatus) {
        songStatus.textContent = statusText;
      }
    }

    function startLoveMeSong() {
      userUnlockedAudio = true;
      if (ytReady && ytPlayer && typeof ytPlayer.playVideo === "function") {
        try {
          if (typeof ytPlayer.unMute === "function") ytPlayer.unMute();
          if (typeof ytPlayer.setVolume === "function") ytPlayer.setVolume(85);
          ytPlayer.playVideo();
          updateSongUI(true, "🎵 Love Me: ON", "🎶 Playing: JMSN — Love Me");
          return;
        } catch (_) {}
      }
      updateSongUI(true, "🎵 Love Me: Starting...", "🎶 Loading JMSN — Love Me...");
    }

    function pauseLoveMeSong() {
      if (ytReady && ytPlayer && typeof ytPlayer.pauseVideo === "function") {
        try {
          ytPlayer.pauseVideo();
        } catch (_) {}
      }
      updateSongUI(false, "🎵 Love Me: PAUSED", "⏸️ Paused: JMSN — Love Me (tap to resume)");
    }

    window.onYouTubeIframeAPIReady = function () {
      if (!window.YT || !window.YT.Player) return;
      ytPlayer = new YT.Player("yt-love-me-player", {
        height: "1",
        width: "1",
        videoId: LOVE_ME_VIDEO_ID,
        playerVars: {
          autoplay: 1,
          loop: 1,
          playlist: LOVE_ME_VIDEO_ID,
          playsinline: 1,
          controls: 0,
          rel: 0
        },
        events: {
          onReady: function (event) {
            ytReady = true;
            try {
              event.target.setVolume(85);
              event.target.playVideo();
            } catch (_) {}
          },
          onStateChange: function (event) {
            if (event.data === YT.PlayerState.PLAYING) {
              updateSongUI(true, "🎵 Love Me: ON", "🎶 Playing: JMSN — Love Me");
            } else if (event.data === YT.PlayerState.PAUSED) {
              updateSongUI(false, "🎵 Love Me: PAUSED", "⏸️ Paused: JMSN — Love Me (tap to resume)");
            } else if (event.data === YT.PlayerState.CUED || event.data === YT.PlayerState.UNSTARTED) {
              if (!userUnlockedAudio) {
                updateSongUI(false, "🎵 Love Me: Tap to Play", "🎶 Tap anywhere or button to start JMSN — Love Me");
              }
            }
          }
        }
      });
    };

    const ytScript = document.createElement("script");
    ytScript.src = "https://www.youtube.com/iframe_api";
    document.head.appendChild(ytScript);

    musicBtn.addEventListener("click", (e) => {
      e.stopPropagation();
      if (!isLoveMePlaying) {
        startLoveMeSong();
      } else {
        pauseLoveMeSong();
      }
    });

    // Unlock audio on first touch/pointer interaction for mobile browsers
    function unlockOnFirstInteraction() {
      if (!userUnlockedAudio) {
        userUnlockedAudio = true;
        startLoveMeSong();
      }
      document.removeEventListener("pointerdown", unlockOnFirstInteraction);
      document.removeEventListener("touchstart", unlockOnFirstInteraction);
    }

    document.addEventListener("pointerdown", unlockOnFirstInteraction, { passive: true });
    document.addEventListener("touchstart", unlockOnFirstInteraction, { passive: true });
  </script>
</body>
</html>