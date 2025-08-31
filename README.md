<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Happy Birthday, Harshita 🎂✨</title>
  <!-- Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700;800&display=swap" rel="stylesheet">
  <link href="https://fonts.cdnfonts.com/css/satoshi" rel="stylesheet">
  <!-- TailwindCSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Three.js -->
  <script src="https://unpkg.com/three@0.158.0/build/three.min.js"></script>
  <!-- GSAP -->
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/ScrollToPlugin.min.js"></script>
  <!-- canvas-confetti -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.3/dist/confetti.browser.min.js"></script>
  <style>
    :root{
      --bg:#0c0a09;
      --glass: rgba(255,255,255,0.06);
      --border: rgba(255,255,255,0.2);
      --text:#eaeaea;
      --pink:#ff3d7b;
      --red:#ff1744;
    }
    html,body{height:100%;background:var(--bg);} 

    /* Aurora Background */
    #aurora{
      position:fixed;inset:0;overflow:hidden;z-index:-2;background:var(--bg);
    }
    #aurora .blob{
      position:absolute;width:70vmax;height:70vmax;filter:blur(80px);opacity:.55;mix-blend:screen;border-radius:50%;
      background:radial-gradient(circle at 30% 30%, hsla(333,70%,55%,.4), transparent 60%);
      animation: drift 22s ease-in-out infinite;
    }
    #aurora .blob:nth-child(2){
      background:radial-gradient(circle at 70% 40%, hsla(282,82%,54%,.3), transparent 60%);
      animation-duration: 28s;animation-delay:-5s;
    }
    #aurora .blob:nth-child(3){
      background:radial-gradient(circle at 40% 70%, hsla(210,89%,60%,.3), transparent 60%);
      animation-duration: 30s;animation-delay:-12s;
    }
    #aurora .blob:nth-child(4){
      background:radial-gradient(circle at 60% 60%, hsla(35,90%,60%,.3), transparent 60%);
      animation-duration: 26s;animation-delay:-8s;
    }
    @keyframes drift{
      0%{transform:translate(-10%, -10%) scale(1);}
      25%{transform:translate(20%, -15%) scale(1.1);}
      50%{transform:translate(15%, 10%) scale(0.95);}
      75%{transform:translate(-15%, 15%) scale(1.08);}
      100%{transform:translate(-10%, -10%) scale(1);} 
    }

    /* ThreeJS canvas layer */
    #bg3d{position:fixed;inset:0;z-index:-1;}

    /* Progress bar */
    .progress-wrap{position:fixed;top:16px;left:50%;transform:translateX(-50%);width:min(680px,84vw);z-index:30;}
    .progress-track{backdrop-filter: blur(8px);-webkit-backdrop-filter: blur(8px);background:rgba(255,255,255,0.06);border:1px solid rgba(255,255,255,0.16);height:8px;border-radius:999px;box-shadow:0 8px 24px rgba(0,0,0,.25);} 
    .progress-bar{height:100%;width:0%;border-radius:999px;background:linear-gradient(90deg, #ff7cab, #ff1744);box-shadow:0 0 18px rgba(255, 50, 120, .6);} 

    /* Cards */
    .card{backdrop-filter: blur(14px);-webkit-backdrop-filter: blur(14px);background:var(--glass);border:1px solid var(--border);box-shadow:0 20px 60px rgba(0,0,0,.45);border-radius:28px;}
    .title{font-family:'Playfair Display', serif; background:linear-gradient(90deg, #ffe1ee, #ffd6e8, #ffffff);-webkit-background-clip:text;background-clip:text;color:transparent;}
    .body{font-family:'Satoshi', system-ui, -apple-system, Segoe UI, Roboto, Inter, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji", "Segoe UI Emoji";color:var(--text);} 

    /* Buttons */
    .btn{
      font-family:'Satoshi', system-ui, -apple-system, Segoe UI, Roboto, Inter;
      background:linear-gradient(135deg, #ff6aa2, #ff1744);
      color:white;border:none;border-radius:999px;padding:14px 26px;font-weight:700;letter-spacing:.3px;box-shadow:0 8px 24px rgba(255, 26, 102, .35), 0 0 0 0 rgba(255, 26, 102, .55) inset;transition: transform .18s ease, box-shadow .18s ease, filter .18s ease; 
    }
    .btn:hover{transform:translateY(-2px) scale(1.05);box-shadow:0 16px 32px rgba(255, 26, 102, .45), 0 0 0 4px rgba(255, 255, 255, .06) inset;filter:saturate(1.1);} 
    .btn:active{transform:translateY(0) scale(0.98);box-shadow:0 6px 16px rgba(255, 26, 102, .35), 0 0 0 2px rgba(255,255,255,.06) inset;} 
    .btn:disabled{opacity:.6;cursor:not-allowed;}

    /* Polaroid */
    .polaroid{width:min(340px,78vw);background:#fff;border-radius:10px;box-shadow:0 24px 50px rgba(0,0,0,.45);padding:12px 12px 18px;transform:rotate(-3deg) perspective(800px);transition:transform .25s ease;}
    .polaroid img{width:100%;height:auto;border-radius:6px;display:block;}
    .polaroid .cap{font-family:'Satoshi', system-ui; text-align:center;margin-top:10px;color:#111;font-weight:700;}
    .polaroid:hover{transform:rotate(0deg) perspective(800px) translateZ(8px);} 

    /* Emoji beat/glow */
    .beat{animation: beat 1.7s ease-in-out infinite; filter: drop-shadow(0 0 12px rgba(255, 61, 123, .55));}
    @keyframes beat{ 0%,100%{transform:scale(1);} 50%{transform:scale(1.12);} }

    /* Emoji streamer container */
    #emoji-layer{position:fixed;inset:0;pointer-events:none;z-index:40;}
    .emoji{position:fixed;font-size:18px;will-change: transform, opacity;}

    /* Step container transitions */
    .step{opacity:0; transform: translateY(12px) scale(.98); display:none;}
    .step.active{display:block;}

    /* Utility */
    .center-wrap{min-height:100vh;display:grid;place-items:center;padding:24px;}
  </style>
</head>
<body class="body">
  <!-- Aurora background layer -->
  <div id="aurora" aria-hidden="true">
    <div class="blob" style="top:-10%; left:-10%"></div>
    <div class="blob" style="bottom:-15%; right:-15%"></div>
    <div class="blob" style="top:20%; right:-20%"></div>
    <div class="blob" style="bottom:10%; left:10%"></div>
  </div>

  <!-- 3D Hearts background -->
  <canvas id="bg3d" aria-hidden="true"></canvas>

  <!-- Progress Bar -->
  <div class="progress-wrap">
    <div class="progress-track">
      <div id="progressBar" class="progress-bar"></div>
    </div>
  </div>

  <!-- Emoji layer for finale -->
  <div id="emoji-layer"></div>

  <!-- Steps Wrapper -->
  <main class="center-wrap">
    <!-- Step 1 -->
    <section id="step1" class="step active">
      <div class="card max-w-3xl mx-auto p-8 md:p-12 text-center">
        <div class="text-6xl md:text-7xl mb-6 beat">❤️</div>
        <h1 class="title text-4xl md:text-6xl font-bold mb-4">Happy Birthday, Harshita!</h1>
        <p class="text-lg md:text-xl text-white/80 mb-8">I built a little world f or you, just to bring a smile to your face on your special day.</p>
        <button class="btn" data-next="2">Let's Begin</button>
      </div>
    </section>

    <!-- Step 2 -->
    <section id="step2" class="step">
      <div class="card max-w-3xl mx-auto p-8 md:p-12 text-center">
        <div class="text-6xl md:text-7xl mb-6">🎉</div>
        <h2 class="title text-3xl md:text-5xl font-bold mb-4">Wishing you a Happiest Birthday!</h2>
        <p class="text-lg md:text-xl text-white/80 mb-8">Another year of you making the world brighter. I was going to get you something expensive, but then I remembered my Budget and now you get this Awesome message instead. You're welcome!</p>
        <button class="btn" data-next="3">There's more...</button>
      </div>
    </section>

    <!-- Step 3 -->
    <section id="step3" class="step">
      <div class="card max-w-5xl mx-auto p-6 md:p-10">
        <h2 class="title text-3xl md:text-5xl font-bold text-center mb-8">A Few Things I Adore About You</h2>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 md:gap-6">
          <div class="col-span-1 md:col-span-2 card p-6 md:p-8 bg-white/5 border-white/15">
            <h3 class="title text-2xl md:text-3xl mb-2">✨ Your Unmatched Kindness</h3>
            <p class="text-white/85">The genuine warmth you show to everyone is something truly rare and beautiful.</p>
          </div>
          <div class="col-span-1 card p-6 md:p-8 bg-white/5 border-white/15">
            <h3 class="title text-2xl md:text-3xl mb-2">😊 That Smile</h3>
            <p class="text-white/85">It's a work of art.</p>
          </div>
          <div class="col-span-1 md:col-span-2 card p-6 md:p-8 bg-white/5 border-white/15">
            <h3 class="title text-2xl md:text-3xl mb-2">🌟 Your Radiant Spirit</h3>
            <p class="text-white/85">Your passion for life is infectious. Being around you makes everything feel more exciting and possible.</p>
          </div>
        </div>
        <div class="text-center mt-8">
          <button class="btn" data-next="4">Remember this?</button>
        </div>
      </div>
    </section>

    <!-- Step 4 -->
    <section id="step4" class="step">
      <div class="card max-w-4xl mx-auto p-8 md:p-12 text-center">
        <h2 class="title text-3xl md:text-5xl font-bold mb-8">That One Time...</h2>
        <div class="flex justify-center mb-8">
          <div id="polaroid" class="polaroid">
            <img src="https://photos.fife.usercontent.google.com/pw/AP1GczM6Cj39NJqB7gO971WxKia0GcZvNNjwLC2oEw9Xzv8XtAoV3K5Q9HPF=w532-h945-s-no-gm?authuser=0" alt="Memory" />
            <div class="cap">Our favorite memory.</div>
          </div>
        </div>
        <p class="text-lg md:text-xl text-white/85 mb-8">Being with you isn't just a moment- it's the best part of my day.</p>
        <button class="btn" data-next="5">One last thing...</button>
      </div>
    </section>

    <!-- Step 5 -->
    <section id="step5" class="step">
      <div class="card max-w-3xl mx-auto p-8 md:p-12 text-center">
        <div class="text-6xl md:text-7xl mb-6">🎂</div>
        <h2 class="title text-3xl md:text-5xl font-bold mb-4">Few Wishes For You</h2>
        <p class="text-base md:text-lg text-white/85 leading-7 md:leading-8 mb-8 whitespace-pre-line">On your special day, I just want to say that you’re not only a wonderful person but also someone who makes life brighter for the people around you. You’re stepping into a new year full of dreams, happiness, and success.
Stay the same cheerful, caring, and amazing girl you are. May this year bring you endless smiles, sweet memories, and everything your heart wishes for.
Always remember—you are really important in my life, and I’ll always be there whenever you need. 💖
🎂🥳Happiest Birthday once again🥳🎂</p>
        <div id="finalMessage" class="title text-3xl md:text-5xl font-bold opacity-0 translate-y-3 mb-6">Happy Birthday ❤️</div>
        <button id="celebrateBtn" class="btn" data-next="final">Celebrate!</button>
      </div>
    </section>
  </main>

  <script>
    // --- Tailwind config (optional customizations) ---
    tailwind.config = { theme: { extend: { boxShadow: { glow: '0 0 30px rgba(255,26,102,.55)' } } } };

    // --- Step management with GSAP ---
    const steps = [1,2,3,4,5];
    let current = 1;
    const progressBar = document.getElementById('progressBar');

    function showStep(n){
      const prev = document.querySelector('.step.active');
      const next = document.getElementById('step'+n);
      if(prev===next) return;

      // Animate out previous
      if(prev){
        gsap.to(prev, { duration:.35, opacity:0, y:12, scale:0.98, onComplete: ()=>{
          prev.classList.remove('active'); prev.style.display='none';
          // Animate in next
          next.style.display='block'; next.classList.add('active');
          gsap.fromTo(next, {opacity:0, y:12, scale:.98}, {duration:.5, opacity:1, y:0, scale:1, ease:'power2.out'});
          // Stagger inner elements
          const ins = next.querySelectorAll('h1,h2,h3,p,button,.polaroid,.grid');
          gsap.fromTo(ins, {opacity:0, y:10}, {opacity:1, y:0, duration:.6, stagger:.06, ease:'power2.out', delay:.05});
        }});
      } else {
        next.style.display='block'; next.classList.add('active');
        gsap.to(next, { duration:.45, opacity:1, y:0, scale:1});
      }
      current = n;
      updateProgress();
    }

    function updateProgress(){
      const idx = steps.indexOf(current)+1; // 1..5
      const pct = (idx-1)/(steps.length-1)*100; // 0..100
      progressBar.style.width = pct + '%';
    }

    document.querySelectorAll('[data-next]').forEach(btn=>{
      btn.addEventListener('click', (e)=>{
        const target = e.currentTarget.getAttribute('data-next');
        if(target === 'final') { startFinale(); return; }
        const n = parseInt(target,10); showStep(n);
      });
    });

    // Initialize first step visibility
    gsap.set('#step1', {opacity:1, y:0, scale:1});
    updateProgress();

    // --- Polaroid parallax hover ---
    (function(){
      const polaroid = document.getElementById('polaroid');
      if(!polaroid) return;
      polaroid.addEventListener('mousemove', (e)=>{
        const rect = polaroid.getBoundingClientRect();
        const cx = rect.left + rect.width/2; const cy = rect.top + rect.height/2;
        const dx = (e.clientX - cx)/rect.width; const dy = (e.clientY - cy)/rect.height;
        const rotX = dy * -6; const rotY = dx * 8;
        polaroid.style.transform = `perspective(900px) rotateX(${rotX}deg) rotateY(${rotY}deg) translateZ(10px)`;
      });
      polaroid.addEventListener('mouseleave', ()=>{
        polaroid.style.transform = 'rotate(-3deg) perspective(800px)';
      });
    })();

    // --- Finale sequence ---
    let finaleStarted = false;
    function startFinale(){
      if(finaleStarted) return; finaleStarted = true;
      const btn = document.getElementById('celebrateBtn');
      btn.disabled = true; gsap.to(btn, {opacity:0, y:6, duration:.3});

      // Show final message
      gsap.to('#finalMessage', {opacity:1, y:0, duration:.8, ease:'power3.out'});

      // Confetti bursts from corners
      const duration = 5 * 1000; // 5 seconds
      const end = Date.now() + duration;

      // Initial powerful bursts
      confetti({ origin: { x: 0.1, y: 1 }, particleCount: 120, angle: 60, spread: 70, startVelocity: 60 });
      confetti({ origin: { x: 0.9, y: 1 }, particleCount: 120, angle: 120, spread: 70, startVelocity: 60 });

      // Gentle continuous shower
      (function frame(){
        confetti({ particleCount: 6, spread: 70, origin: { x: Math.random(), y: Math.random()*0.2 }, ticks: 250 });
        if(Date.now() < end) requestAnimationFrame(frame);
      })();

      // Emoji confetti (DOM-based)
      const emojis = ['❤️','💖','✨'];
      const emojiLayer = document.getElementById('emoji-layer');
      const emojiEnd = Date.now() + duration;
      (function drop(){
        for(let i=0;i<6;i++){
          const span = document.createElement('span');
          span.className = 'emoji'; span.textContent = emojis[Math.floor(Math.random()*emojis.length)];
          const startX = Math.random() * window.innerWidth;
          span.style.left = startX + 'px'; span.style.top = '-24px';
          emojiLayer.appendChild(span);
          const drift = (Math.random()*60 - 30);
          gsap.to(span, { y: window.innerHeight + 60, x: `+=${drift}`, rotation: Math.random()*360, duration: 3.5 + Math.random()*1.5, ease: 'power1.in', onComplete(){ span.remove(); } });
        }
        if(Date.now() < emojiEnd) setTimeout(drop, 180);
      })();

      // Hearts swirl out and vanish
      heartsFinale();
    }

    // --- THREE.js: floating hearts background ---
    const canvas = document.getElementById('bg3d');
    const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(55, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 24;

    const amb = new THREE.AmbientLight(0xffffff, 0.75); scene.add(amb);
    const dir = new THREE.DirectionalLight(0xffffff, 1.0); dir.position.set(5,10,7); scene.add(dir);

    function makeHeartGeometry(){
      const x=0, y=0; const heartShape = new THREE.Shape();
      heartShape.moveTo(x + 0, y + 0);
      heartShape.bezierCurveTo(0, 0, -3, -3, -3, -5);
      heartShape.bezierCurveTo(-3, -8, 0, -9.5, 0, -12);
      heartShape.bezierCurveTo(0, -9.5, 3, -8, 3, -5);
      heartShape.bezierCurveTo(3, -3, 0, 0, 0, 0);
      const extrudeSettings = { depth: 1.2, bevelEnabled: true, bevelSegments: 2, steps: 2, bevelSize: .4, bevelThickness: .4 };
      return new THREE.ExtrudeGeometry(heartShape, extrudeSettings);
    }

    const heartGeo = makeHeartGeometry();
    const pinks = [0xff5a9e, 0xff2e63, 0xff6ab3, 0xff1744, 0xff3d7b];
    const hearts = [];
    const HEART_COUNT = 22; // 20-25

    for(let i=0;i<HEART_COUNT;i++){
      const mat = new THREE.MeshPhongMaterial({ color: pinks[Math.floor(Math.random()*pinks.length)], shininess: 80, specular: 0xffffff, transparent:true, opacity:0.9 });
      const mesh = new THREE.Mesh(heartGeo, mat);
      const s = 0.3 + Math.random()*0.7; mesh.scale.set(s,s,s);
      mesh.position.set((Math.random()-0.5)*26, (Math.random()-0.5)*14, (Math.random()-0.5)*-20);
      mesh.rotation.set(Math.random()*Math.PI, Math.random()*Math.PI, Math.random()*Math.PI);
      mesh.userData = { baseX: mesh.position.x, baseY: mesh.position.y, phase: Math.random()*Math.PI*2, speed: .5 + Math.random()*0.8, swirl:false };
      scene.add(mesh); hearts.push(mesh);
    }

    function onResize(){
      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      const w = window.innerWidth, h = window.innerHeight;
      renderer.setPixelRatio(dpr); renderer.setSize(w,h,false);
      camera.aspect = w/h; camera.updateProjectionMatrix();
    }
    window.addEventListener('resize', onResize); onResize();

    let t = 0;
    function animate(){
      requestAnimationFrame(animate); t += 0.01;
      hearts.forEach((h,i)=>{
        if(!h.userData.swirl){
          h.position.y = h.userData.baseY + Math.sin(t * h.userData.speed + h.userData.phase) * 0.6;
          h.position.x = h.userData.baseX + Math.cos(t * (h.userData.speed*0.8) + h.userData.phase) * 0.4;
          h.rotation.y += 0.002; h.rotation.x += 0.0015;
        }
      });
      renderer.render(scene, camera);
    }
    animate();

    function heartsFinale(){
      hearts.forEach((h, i)=>{
        h.userData.swirl = true;
        const dirX = (h.position.x) * 0.12 + (Math.random()-0.5)*2;
        const dirY = Math.abs(h.position.y) + 6 + Math.random()*3;
        // GSAP animate: move upward and outward, fade
        gsap.to(h.position, { x: h.position.x + dirX*6, y: h.position.y + dirY, z: h.position.z - 10, duration: 3 + Math.random()*1.5, ease:'power2.out' });
        gsap.to(h.rotation, { y: h.rotation.y + (Math.random()>.5?1:-1)*Math.PI*2, x: h.rotation.x + Math.PI, duration: 3.2, ease:'power2.out' });
        gsap.to(h.material, { opacity: 0, duration: 4, ease: 'power1.out', delay:.4 });
      });
    }
  </script>
</body>
</html>
