<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Happy Birthday Harshita 🎂 — An Interactive Greeting</title>

  <!-- Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@600;700;800&family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">

  <!-- Tailwind (CDN for utility convenience) -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Three.js -->
  <script src="https://unpkg.com/three@0.152.2/build/three.min.js"></script>

  <!-- GSAP (core) -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>

  <!-- canvas-confetti -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>

  <style>
    :root{
      --bg-dark:#0c0a09;
      --card-bg: rgba(255,255,255,0.04);
      --glass-border: rgba(255,255,255,0.06);
      --muted: #e6e6e9;
      --gradient-pink: linear-gradient(90deg, #ff7ab6, #ff3b6b);
      --btn-glow: 0 10px 30px rgba(255,66,139,0.15);
      --accent-pink: #ff3b6b;
    }

    html,body{
      height:100%;
      margin:0;
      font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
      background: var(--bg-dark);
      color: var(--muted);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      overflow:hidden;
    }

    /* AURORA LAYERS */
    .aurora {
      position:fixed;
      inset:0;
      background: #0c0a09;
      z-index:0;
      overflow:hidden;
    }
    .aurora::before,
    .aurora::after {
      content: "";
      position:absolute;
      inset:-20%;
      filter: blur(80px) saturate(1.05);
      opacity:0.95;
      mix-blend-mode: screen;
      animation: flow 18s ease-in-out infinite alternate;
      pointer-events:none;
    }
    /* Layered radial gradients */
    .aurora::before {
      background:
        radial-gradient(35% 40% at 10% 20%, hsla(333,70%,55%,.42), transparent 20%),
        radial-gradient(30% 35% at 85% 10%, hsla(282,82%,54%,.30), transparent 20%),
        radial-gradient(25% 30% at 50% 80%, hsla(210,89%,60%,.24), transparent 25%);
      transform: translate3d(0,0,0) rotate(0.001deg);
      animation-duration: 22s;
    }
    .aurora::after {
      background:
        radial-gradient(30% 30% at 15% 80%, hsla(35,90%,60%,.20), transparent 15%),
        radial-gradient(40% 45% at 70% 60%, hsla(333,70%,55%,.22), transparent 18%),
        radial-gradient(28% 32% at 40% 30%, hsla(282,82%,54%,.18), transparent 20%);
      animation-duration: 26s;
      transform-origin: center;
    }
    @keyframes flow {
      0% { transform: translate(-6%, -3%) scale(1) rotate(-4deg); filter: blur(70px) saturate(1.1); }
      50% { transform: translate(6%, 4%) scale(1.05) rotate(3deg); filter: blur(78px) saturate(1.05); }
      100% { transform: translate(-3%, -2%) scale(1) rotate(-2deg); filter: blur(72px) saturate(1.15); }
    }

    /* Three.js canvas sits above aurora but behind content */
    #three-canvas {
      position:fixed;
      inset:0;
      z-index:1;
      pointer-events:none;
    }

    /* Main container */
    .app {
      position:relative;
      z-index:5;
      height:100vh;
      width:100%;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:clamp(16px,3vw,40px);
      box-sizing:border-box;
    }

    /* Progress bar */
    .progress-wrap {
      position:fixed;
      top:18px;
      left:50%;
      transform:translateX(-50%);
      z-index:10;
      width:min(760px,92%);
      max-width:92%;
      display:flex;
      align-items:center;
      justify-content:center;
      pointer-events:none;
    }
    .progress-track {
      width:100%;
      height:8px;
      background:rgba(255,255,255,0.04);
      border-radius:999px;
      backdrop-filter: blur(8px);
      border:1px solid rgba(255,255,255,0.03);
      padding:2px;
      box-sizing:border-box;
    }
    .progress-bar {
      height:100%;
      width:0%;
      border-radius:999px;
      background: linear-gradient(90deg,#ff7ab6,#ff3b6b);
      box-shadow: 0 6px 24px rgba(255,59,107,0.12);
      transition: width 0.6s cubic-bezier(.2,.9,.25,1);
    }

    /* Steps container */
    .steps {
      width:100%;
      max-width:1000px;
      display:grid;
      place-items:center;
      gap:20px;
    }

    .card {
      width:100%;
      max-width:820px;
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border: 1px solid var(--glass-border);
      border-radius:20px;
      padding:clamp(20px,3.5vw,36px);
      backdrop-filter: blur(10px) saturate(1.05);
      box-shadow: 0 10px 40px rgba(2,2,2,0.6);
      color:var(--muted);
      transform-origin:center;
      overflow:visible;
    }

    .card h1, .card h2 {
      font-family: 'Playfair Display', serif;
      margin:0 0 8px 0;
      line-height:1.05;
      font-weight:700;
      background: linear-gradient(90deg, #ffd6e8, #ffc0d9, #fff);
      -webkit-background-clip: text;
      background-clip:text;
      -webkit-text-fill-color: transparent;
      font-size: clamp(22px,3.2vw,36px);
    }

    .card p {
      font-size: clamp(14px,1.8vw,17px);
      color: rgba(230,230,233,0.92);
      margin:8px 0 18px 0;
      line-height:1.5;
      font-family: 'Inter', sans-serif;
    }

    /* Buttons */
    .btn {
      display:inline-flex;
      align-items:center;
      justify-content:center;
      gap:10px;
      padding:10px 18px;
      border-radius:999px;
      font-weight:700;
      cursor:pointer;
      color:white;
      user-select:none;
      border:0;
      outline:0;
      transition: transform 220ms cubic-bezier(.2,.9,.25,1), box-shadow 220ms;
      box-shadow: var(--btn-glow);
      background: var(--gradient-pink);
      transform-origin:center;
      will-change:transform;
    }
    .btn:hover {
      transform: translateY(-6px) scale(1.05);
      box-shadow: 0 18px 50px rgba(255,66,139,0.18);
    }
    .btn:active {
      transform: translateY(-2px) scale(0.99);
      box-shadow: 0 8px 24px rgba(255,66,139,0.12);
    }

    /* Step animations helper classes (initially hidden) */
    .step {
      opacity:0;
      transform: translateY(20px) scale(0.98);
    }

    /* Bento grid for reasons */
    .bento {
      display:grid;
      grid-template-columns: repeat(3,1fr);
      grid-auto-rows: minmax(100px, auto);
      gap:12px;
      margin:14px 0 18px 0;
    }
    .bento .tile {
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:12px;
      padding:12px;
      border:1px solid rgba(255,255,255,0.03);
      display:flex;
      flex-direction:column;
      gap:8px;
      box-shadow: 0 6px 22px rgba(0,0,0,0.45);
    }
    .tile.large {
      grid-column: 1 / span 2;
      grid-row: auto;
    }
    .tile h3 {
      margin:0;
      font-family: 'Playfair Display', serif;
      font-size:18px;
      -webkit-background-clip: text;
      background-clip:text;
      -webkit-text-fill-color: transparent;
      background: linear-gradient(90deg,#ffd6e8,#ffdfe9);
    }
    .tile p {
      margin:0;
      color: rgba(230,230,233,0.9);
      font-size:14px;
    }

    /* Polaroid */
    .polaroid-wrap {
      perspective:1200px;
      display:flex;
      align-items:center;
      justify-content:center;
      margin:18px 0;
    }
    .polaroid {
      width:min(360px,78%);
      max-width:420px;
      background:white;
      border-radius:8px;
      padding:14px 14px 18px 14px;
      box-shadow: 0 20px 40px rgba(2,2,2,0.6);
      transform-origin:center;
      transform: rotate(-6deg);
      transition: transform 300ms ease;
      will-change: transform;
      user-select:none;
    }
    .polaroid .photo {
      width:100%;
      height:220px;
      background-size:cover;
      background-position:center;
      border-radius:6px;
      box-shadow: 0 6px 18px rgba(0,0,0,0.45);
      transform: translateZ(12px);
    }
    .polaroid .caption {
      margin-top:10px;
      font-size:13px;
      color:#333;
      text-align:center;
      font-weight:600;
    }

    /* Finale text area */
    .final {
      text-align:center;
      padding-top:8px;
    }
    .final .big {
      font-family:'Playfair Display',serif;
      font-size: clamp(28px,5.2vw,44px);
      margin:10px 0;
      -webkit-background-clip:text;
      background-clip:text;
      -webkit-text-fill-color:transparent;
      background: linear-gradient(90deg,#ffd7e9,#fff);
    }
    .final .mini {
      font-size:16px;
      color:rgba(230,230,233,0.95);
      margin-bottom:12px;
      line-height:1.5;
    }

    /* Responsive tweaks */
    @media (max-width:720px){
      .card { padding:18px; border-radius:16px; }
      .bento { grid-template-columns: repeat(2,1fr); }
      .tile.large { grid-column: 1 / span 2; }
      .polaroid { transform: rotate(-4deg) scale(0.98); }
    }

    /* small decorative emoji style */
    .big-emoji {
      font-size:64px;
      display:inline-block;
      filter: drop-shadow(0 12px 30px rgba(255,59,107,0.12));
      animation: beat 1.6s ease-in-out infinite;
      transform-origin:center;
      margin-bottom:8px;
    }
    @keyframes beat {
      0% { transform: scale(1); filter: drop-shadow(0 8px 22px rgba(255,59,107,0.08)); }
      50% { transform: scale(1.08); filter: drop-shadow(0 18px 36px rgba(255,59,107,0.18)); }
      100% { transform: scale(1); filter: drop-shadow(0 8px 22px rgba(255,59,107,0.08)); }
    }

    /* subtle focus styles */
    .btn:focus { box-shadow: 0 0 0 4px rgba(255,59,107,0.08); outline: none; }

    /* small credit / hint */
    .hint {
      position:fixed;
      bottom:16px;
      right:18px;
      z-index:12;
      color:rgba(255,255,255,0.5);
      font-size:12px;
      backdrop-filter: blur(6px);
      padding:6px 8px;
      border-radius:8px;
      border:1px solid rgba(255,255,255,0.03);
    }
  </style>
</head>
<body>

  <!-- Aurora background -->
  <div class="aurora" aria-hidden="true"></div>

  <!-- Three.js canvas for floating hearts -->
  <canvas id="three-canvas"></canvas>

  <!-- Progress -->
  <div class="progress-wrap" aria-hidden="true">
    <div class="progress-track" role="progressbar" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0">
      <div id="progress-bar" class="progress-bar" style="width:0%"></div>
    </div>
  </div>

  <!-- App Content -->
  <main class="app" id="app">
    <div class="steps" id="steps">

      <!-- Step 1 -->
      <section class="card step" id="step-1" data-step="1" role="region" aria-labelledby="s1-title">
        <div style="display:flex; align-items:center; gap:16px; flex-wrap:wrap;">
          <div>
            <div class="big-emoji" id="heart-emoji" aria-hidden="true">❤️</div>
          </div>
          <div style="flex:1; min-width:220px;">
            <h1 id="s1-title">Happy Birthday, Harshita!</h1>
            <p>I built a little world for you, just to bring a smile to your face on your special day.</p>
            <div style="display:flex; gap:10px; flex-wrap:wrap;">
              <button class="btn" id="btn-1">Let's Begin</button>
            </div>
          </div>
        </div>
      </section>

      <!-- Step 2 -->
      <section class="card step" id="step-2" data-step="2" role="region" aria-labelledby="s2-title" style="display:none;">
        <div style="display:flex; align-items:center; gap:16px; flex-wrap:wrap;">
          <div>
            <div class="big-emoji" aria-hidden="true">🎉</div>
          </div>
          <div style="flex:1; min-width:220px;">
            <h1 id="s2-title">Wishing you a Happiest Birthday!</h1>
            <p>Another year of you making the world brighter. I was going to get you something expensive, but then I remembered my Budget and now you get this Awesome message instead. You're welcome!</p>
            <div style="display:flex; gap:10px; flex-wrap:wrap;">
              <button class="btn" id="btn-2">There's more...</button>
            </div>
          </div>
        </div>
      </section>

      <!-- Step 3 -->
      <section class="card step" id="step-3" data-step="3" role="region" aria-labelledby="s3-title" style="display:none;">
        <h2 id="s3-title">A Few Things I Adore About You</h2>
        <div class="bento" aria-hidden="false">
          <div class="tile large" data-anim>
            <h3>✨ Your Unmatched Kindness</h3>
            <p>The genuine warmth you show to everyone is something truly rare and beautiful.</p>
          </div>
          <div class="tile" data-anim>
            <h3>😊 That Smile</h3>
            <p>It's a work of art.</p>
          </div>
          <div class="tile large" data-anim>
            <h3>🌟 Your Radiant Spirit</h3>
            <p>Your passion for life is infectious. Being around you makes everything feel more exciting and possible.</p>
          </div>
        </div>
        <div style="display:flex; gap:10px;">
          <button class="btn" id="btn-3">Remember this?</button>
        </div>
      </section>

      <!-- Step 4 -->
      <section class="card step" id="step-4" data-step="4" role="region" aria-labelledby="s4-title" style="display:none;">
        <h2 id="s4-title">That One Time...</h2>
        <div class="polaroid-wrap" id="polaroid-wrap" aria-hidden="false">
          <div class="polaroid" id="polaroid">
            <div class="photo" id="polaroid-photo" style="background-image:url('https://i.ibb.co/6Z6XgCg/crush.webp');"></div>
            <div class="caption">Our favorite memory.</div>
          </div>
        </div>
        <p>Being with you isn't just a moment—it's the best part of my day.</p>
        <div style="display:flex; gap:10px;">
          <button class="btn" id="btn-4">One last thing...</button>
        </div>
      </section>

      <!-- Step 5 -->
      <section class="card step" id="step-5" data-step="5" role="region" aria-labelledby="s5-title" style="display:none;">
        <div style="display:flex;align-items:center;gap:16px;flex-wrap:wrap;">
          <div>
            <div class="big-emoji" aria-hidden="true">🎂</div>
          </div>
          <div style="flex:1;min-width:220px;">
            <h2 id="s5-title">Few Wishes For You</h2>
            <p>On your special day, I just want to say that you’re not only a wonderful person but also someone who makes life brighter for the people around you. You’re stepping into a new year full of dreams, happiness, and success.
Stay the same cheerful, caring, and amazing girl you are. May this year bring you endless smiles, sweet memories, and everything your heart wishes for.
Always remember—you are really important in my life, and I’ll always be there whenever you need. 💖
🎂🥳Happiest Birthday once again🥳🎂</p>

            <div style="display:flex;gap:10px;align-items:center;">
              <button class="btn" id="btn-5">Celebrate!</button>
            </div>

            <div class="final" id="final-area" style="margin-top:18px;opacity:0;">
              <!-- final content will be revealed during finale -->
              <div class="big" id="final-big" aria-hidden="true"></div>
              <div class="mini" id="final-mini" aria-hidden="true"></div>
            </div>

          </div>
        </div>
      </section>

    </div>
  </main>

  <div class="hint" aria-hidden="true">Tip: click the buttons to move through the surprise ✨</div>

  <script>
    /**************************************************************
     * Utility & Animation Setup
     **************************************************************/
    const steps = Array.from(document.querySelectorAll('.step'));
    let current = 0;
    const totalSteps = 5;
    const progressBar = document.getElementById('progress-bar');

    // helper to set progress
    function setProgress(stepIndex){
      const percent = Math.round(((stepIndex+1)/totalSteps) * 100);
      progressBar.style.width = percent + '%';
      progressBar.setAttribute('aria-valuenow', percent);
    }

    // animate step in
    function showStep(index){
      // bounds
      index = Math.max(0, Math.min(index, steps.length - 1));
      const prev = current;
      if(index === prev) return;
      const prevCard = steps[prev];
      const nextCard = steps[index];

      // If prev exists, animate it out
      if(prevCard) {
        gsap.to(prevCard, { duration:0.45, opacity:0, scale:0.98, y:20, ease:"power2.inOut", onComplete:()=>{
          prevCard.style.display='none';
        }});
      }

      // Prepare next
      nextCard.style.display='block';
      gsap.fromTo(nextCard, {opacity:0, y:20, scale:0.98}, {duration:0.6, opacity:1, y:0, scale:1, ease:"power3.out"});

      // animate inner children slightly staggered
      gsap.fromTo(nextCard.querySelectorAll('h1,h2,h3,p,.btn,.tile,.polaroid'), {y:10, opacity:0}, {y:0, opacity:1, duration:0.55, stagger:0.06, delay:0.08, ease:"power2.out"});

      // progress
      current = index;
      setProgress(current);
    }

    // Attach button listeners
    document.getElementById('btn-1').addEventListener('click', ()=> showStep(1));
    document.getElementById('btn-2').addEventListener('click', ()=> showStep(2));
    document.getElementById('btn-3').addEventListener('click', ()=> showStep(3));
    document.getElementById('btn-4').addEventListener('click', ()=> showStep(4));

    // initialize first step
    window.addEventListener('load', ()=>{
      // set initial visible step
      steps.forEach((s,i)=> { if(i!==0) s.style.display='none'; });
      // initial animate first card
      gsap.fromTo(steps[0], {opacity:0, y:20, scale:0.98}, {opacity:1,y:0,scale:1, duration:0.8, ease:"power3.out"});
      gsap.fromTo('#heart-emoji',{scale:0.92, opacity:0},{scale:1, opacity:1, duration:0.9, ease:"elastic.out(1,0.6)"});
      setProgress(0);
    });

    /**************************************************************
     * Bento tiles subtle float
     **************************************************************/
    gsap.utils.toArray('.bento .tile').forEach((el, i)=>{
      gsap.fromTo(el, {y:6}, {y:-6, duration:4 + (i%3), repeat:-1, yoyo:true, ease:'sine.inOut'});
    });

    /**************************************************************
     * Polaroid interactive tilt
     **************************************************************/
    const polaroid = document.getElementById('polaroid');
    const polaroidWrap = document.getElementById('polaroid-wrap');

    polaroidWrap.addEventListener('pointermove', (e)=>{
      const rect = polaroidWrap.getBoundingClientRect();
      const px = (e.clientX - rect.left) / rect.width;
      const py = (e.clientY - rect.top) / rect.height;
      const rx = (py - 0.5) * 12;
      const ry = (px - 0.5) * -18;
      gsap.to(polaroid, {rotationX: rx, rotationY: ry, rotationZ: -6 + (px-0.5)*2, transformPerspective:1200, duration:0.45, ease:"power3.out"});
    });
    polaroidWrap.addEventListener('pointerleave', ()=> {
      gsap.to(polaroid, {rotationX:0, rotationY:0, rotationZ:-6, duration:0.6, ease:"power3.out"});
    });

    /**************************************************************
     * THREE.JS - Floating 3D Hearts
     **************************************************************/
    const canvas = document.getElementById('three-canvas');
    const renderer = new THREE.WebGLRenderer({canvas, alpha:true, antialias:true});
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    const scene = new THREE.Scene();

    // camera
    const camera = new THREE.PerspectiveCamera(40, window.innerWidth / window.innerHeight, 0.1, 2000);
    camera.position.set(0, 0, 80);

    // lights
    const amb = new THREE.AmbientLight(0xffffff, 0.6);
    scene.add(amb);
    const dir = new THREE.DirectionalLight(0xffffff, 0.6);
    dir.position.set(40, 60, 80);
    scene.add(dir);

    // heart geometry (extruded 2D heart shape)
    function createHeartShape(){
      const x = 0, y = 0;
      const heartShape = new THREE.Shape();
      heartShape.moveTo(x, y + 5);
      heartShape.bezierCurveTo(x, y + 5, x - 5, y, x - 5, y - 3);
      heartShape.bezierCurveTo(x - 5, y - 8, x, y - 11, x, y - 14);
      heartShape.bezierCurveTo(x, y - 11, x + 5, y - 8, x + 5, y - 3);
      heartShape.bezierCurveTo(x + 5, y, x, y + 5, x, y + 5);
      return heartShape;
    }

    const hearts = [];
    const heartShape = createHeartShape();
    const extrudeSettings = { depth: 2.6, bevelEnabled: true, bevelSegments: 2, steps: 2, bevelSize: 0.9, bevelThickness: 0.6 };

    const heartCount = 22;
    for(let i=0;i<heartCount;i++){
      const geom = new THREE.ExtrudeGeometry(heartShape, extrudeSettings);
      geom.rotateX(Math.PI); // orient
      geom.scale(0.18, 0.18, 0.18);
      // center geometry
      geom.computeBoundingBox();
      const bbox = geom.boundingBox;
      const centerX = (bbox.max.x + bbox.min.x)/2;
      const centerY = (bbox.max.y + bbox.min.y)/2;
      const centerZ = (bbox.max.z + bbox.min.z)/2;
      geom.translate(-centerX, -centerY, -centerZ);

      const hue = 340 + Math.random()*20; // pink-red
      const color = new THREE.Color(`hsl(${hue}, 80%, ${48 - Math.random()*6}%)`);
      const mat = new THREE.MeshStandardMaterial({color: color, metalness: 0.25, roughness: 0.3, emissive: color.clone().multiplyScalar(0.08)});
      const mesh = new THREE.Mesh(geom, mat);

      // random position in a 3D space behind content
      mesh.position.x = (Math.random()-0.5) * 120;
      mesh.position.y = (Math.random()-0.5) * 60;
      mesh.position.z = -Math.random() * 140; // varied depth
      mesh.rotation.set(Math.random()*Math.PI, Math.random()*Math.PI, Math.random()*Math.PI);
      const scale = 0.7 + Math.random()*1.8;
      mesh.scale.multiplyScalar(scale);

      scene.add(mesh);
      hearts.push({
        mesh,
        baseY: mesh.position.y,
        speed: 0.2 + Math.random()*0.9,
        drift: (Math.random()-0.5)*0.6
      });
    }

    // resize
    function onResize(){
      const w = window.innerWidth;
      const h = window.innerHeight;
      renderer.setSize(w, h);
      camera.aspect = w/h;
      camera.updateProjectionMatrix();
    }
    window.addEventListener('resize', onResize, {passive:true});
    onResize();

    // animate hearts gently
    let t = 0;
    let heartsFinalized = false;
    function animateThree(){
      t += 0.016;
      hearts.forEach((h,i)=>{
        // gentle bobbing via sine
        h.mesh.position.y = h.baseY + Math.sin(t * (0.6 + h.speed*0.6) + i) * (3 + h.speed*1.5);
        h.mesh.rotation.y += 0.002 + h.speed * 0.003;
        h.mesh.position.x += Math.sin(t * 0.2 + i) * (0.002 + h.drift*0.04);
      });

      // slight camera drift for life
      camera.position.x = Math.sin(t * 0.03) * 2.3;
      camera.position.y = Math.sin(t * 0.02) * 1.4;

      renderer.render(scene, camera);
      if(!heartsFinalized) requestAnimationFrame(animateThree);
    }
    animateThree();

    /**************************************************************
     * Celebrate Finale Sequence
     **************************************************************/
    const celebrateBtn = document.getElementById('btn-5');
    const finalArea = document.getElementById('final-area');
    const finalBig = document.getElementById('final-big');
    const finalMini = document.getElementById('final-mini');

    function runFinale(){
      // disable button visually
      celebrateBtn.disabled = true;
      celebrateBtn.style.opacity = 0.6;
      gsap.to(celebrateBtn, {duration:0.45, scale:0.96, opacity:0.3, pointerEvents:'none', ease:'power2.out'});

      // reveal final message
      gsap.to(finalArea, {duration:0.8, opacity:1, delay:0.25, ease:"power3.out"});
      gsap.fromTo(finalArea, {y:18}, {y:0, duration:0.8, ease:"elastic.out(1,0.6)"});

      // typed-like reveal for big text
      const bigText = "Happy Birthday ❤️";
      const miniText = "Wishing you a year full of wonder, laughter, and everything your heart dreams of. With love, always.";

      // simple char-by-char reveal
      finalBig.textContent = "";
      finalMini.textContent = "";

      let bi = 0;
      function revealBigChar(){
        if(bi < bigText.length){
          finalBig.textContent += bigText[bi++];
          setTimeout(revealBigChar, 55);
        }
      }
      setTimeout(revealBigChar, 350);

      let mi = 0;
      function revealMiniChar(){
        if(mi < miniText.length){
          finalMini.textContent += miniText[mi++];
          setTimeout(revealMiniChar, 18);
        }
      }
      setTimeout(revealMiniChar, 1400);

      // confetti: staged bursts then gentle rain with emojis
      // create a sequence with canvas-confetti
      const confettiDuration = 5 * 1000;
      const end = Date.now() + confettiDuration;

      // powerful corner bursts
      confetti({
        particleCount: 40,
        angle: 60,
        spread: 55,
        origin: { x: 0.0, y: 1.0 },
        scalar: 1.4
      });
      confetti({
        particleCount: 40,
        angle: 120,
        spread: 55,
        origin: { x: 1.0, y: 1.0 },
        scalar: 1.4
      });

      // continuous gentle stream (colors + emoji)
      (function frame(){
        confetti({
          particleCount: 6,
          startVelocity: 18,
          spread: 40,
          origin: { x: Math.random(), y: 0 },
          ticks: 120,
          gravity: 0.35,
          shapes: ['square','circle'],
          colors: ['#ff8ab3','#ffd28a','#9be3ff','#ffd6e9','#ff6b95']
        });

        // occasional emoji pieces using small text canvas trick supported by canvas-confetti
        confetti({
          particleCount: 5,
          angle: 90,
          spread: 55,
          origin: { x: Math.random(), y: 0 },
          scalar: 0.9,
          shapes: ['text'],
          // a few emoji samples:
          // note: library supports drawShapes via options, but many builds accept 'text' with 'emojis' array in some versions.
          // fallback: use colored confetti as above if not supported by runtime.
        });

        if (Date.now() < end) {
          requestAnimationFrame(frame);
        }
      })();

      // Hearts final animation: swirl outwards & up, then fade
      heartsFinalized = false; // stop normal loop and animate custom
      // stop the loop by canceling requestAnimationFrame by not calling animateThree again; instead create a gsap timeline to animate each heart
      const tl = gsap.timeline({
        defaults: { duration: 2.6, ease: "power2.inOut" }
      });

      hearts.forEach((h, i) => {
        // compute random outward vector
        const dirX = (h.mesh.position.x >= 0 ? 1 : -1) * (120 + Math.random()*60);
        const dirY = 60 + Math.random()*120;
        const dirZ = -200 - Math.random()*200;
        tl.to(h.mesh.position, {
          x: h.mesh.position.x + dirX,
          y: h.mesh.position.y + dirY,
          z: dirZ,
          rotationZ: "+=" + (Math.PI * (0.4 + Math.random()*1.6)),
          rotationX: "+=" + (Math.PI * (0.3 + Math.random()*1.2)),
          opacity: 0,
          onUpdate: ()=> { /* three.js will pick up mesh positions */ },
          onComplete: ()=> {
            scene.remove(h.mesh);
          }
        }, Math.random() * 0.6);
      });

      // after timeline end, gently fade aurora and show final shimmer
      tl.to({}, { duration: 0.5, onComplete: () => {
        // light celebration pulse
        gsap.to('.aurora::before', {opacity:0.6}); // won't target pseudo; instead pulse the aurora element
        gsap.to('.aurora', {duration:0.8, opacity:1, ease:"sine.out"});
      }});

      // finally stop three renderer after some seconds to save cpu
      setTimeout(()=>{
        heartsFinalized = true;
        // set a gentle fade out of canvas
        gsap.to(canvas, {duration:1.8, opacity:0, ease:'power2.out'});
      }, 2600);
    }

    celebrateBtn.addEventListener('click', runFinale);

    /**************************************************************
     * Accessibility & Keyboard navigation
     **************************************************************/
    // allow Enter to advance when a button is focused
    document.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter'){
        const active = document.activeElement;
        if(active && active.classList && active.classList.contains('btn')){
          active.click();
        }
      }
      // left/right for previous/next
      if(e.key === 'ArrowRight') {
        showStep(Math.min(current+1, steps.length-1));
      } else if(e.key === 'ArrowLeft') {
        showStep(Math.max(current-1, 0));
      }
    });

    /**************************************************************
     * Small polishes & safety: ensure content is responsive & accessible
     **************************************************************/
    // Ensure touch-friendly interactions for mobile: slightly larger hit zones
    document.querySelectorAll('.btn').forEach(b=>{
      b.style.touchAction = 'manipulation';
    });

  </script>
</body>
</html>
