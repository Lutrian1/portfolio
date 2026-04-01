<script>
    import { LandingPageSection } from '$lib';
    import { onMount } from 'svelte';
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';
    import * as ogl from 'ogl'; // Zorg dat je 'ogl' hebt geïnstalleerd: npm install ogl

    gsap.registerPlugin(ScrollTrigger);

    const splitText = (text) => text.split("");

    let canvasContainer;

    onMount(() => {
        // --- 1. OGL FLOWMAP SETUP ---
        const vertex = `
            attribute vec2 uv;
            attribute vec2 position;
            varying vec2 vUv;
            void main() {
                vUv = uv;
                gl_Position = vec4(position, 0, 1);
            }
        `;
        const fragment = `
            precision highp float;
            precision highp int;
            uniform sampler2D tWater;
            uniform sampler2D tFlow;
            uniform float uTime;
            varying vec2 vUv;
            uniform vec4 res;

            void main() {
                vec3 flow = texture2D(tFlow, vUv).rgb;
                vec2 uv = .5 * gl_FragCoord.xy / res.xy;
                
                vec2 myUV = (uv - vec2(0.5)) * res.zw + vec2(0.5);
                myUV -= flow.xy * 0.18;

                vec2 myUV2 = (uv - vec2(0.5)) * res.zw + vec2(0.5);
                myUV2 -= flow.xy * 0.15;

                vec2 myUV3 = (uv - vec2(0.5)) * res.zw + vec2(0.5);
                myUV3 -= flow.xy * 0.12;

                vec3 tex = texture2D(tWater, myUV).rgb;
                vec3 tex2 = texture2D(tWater, myUV2).rgb;
                vec3 tex3 = texture2D(tWater, myUV3).rgb;

                gl_FragColor = vec4(tex.r, tex2.g, tex3.b, 1.0);
            }
        `;

        const renderer = new ogl.Renderer({ dpr: 2, alpha: true });
        const gl = renderer.gl;
        canvasContainer.appendChild(gl.canvas);

        const aspect = 1;
        const mouse = new ogl.Vec2(-1);
        const velocity = new ogl.Vec2();
        const _size = [2048, 1638];

        function resize() {
            renderer.setSize(window.innerWidth, window.innerHeight);
            let a1, a2;
            const imageAspect = _size[1] / _size[0];
            if (window.innerHeight / window.innerWidth < imageAspect) {
                a1 = 1;
                a2 = window.innerHeight / window.innerWidth / imageAspect;
            } else {
                a1 = window.innerWidth / window.innerHeight * imageAspect;
                a2 = 1;
            }
            mesh.program.uniforms.res.value = new ogl.Vec4(window.innerWidth, window.innerHeight, a1, a2);
        }

        const flowmap = new ogl.Flowmap(gl, { falloff: 0.3, dissipation: 0.92 });
        const geometry = new ogl.Geometry(gl, {
            position: { size: 2, data: new Float32Array([-1, -1, 3, -1, -1, 3]) },
            uv: { size: 2, data: new Float32Array([0, 0, 2, 0, 0, 2]) }
        });

        const texture = new ogl.Texture(gl, { minFilter: gl.LINEAR, magFilter: gl.LINEAR });
        const img = new Image();
        img.crossOrigin = "Anonymous";
        img.src = "https://robindelaporte.fr/codepen/bg3.jpg";
        img.onload = () => (texture.image = img);

        const program = new ogl.Program(gl, {
            vertex,
            fragment,
            uniforms: {
                uTime: { value: 0 },
                tWater: { value: texture },
                res: { value: new ogl.Vec4(window.innerWidth, window.innerHeight, 1, 1) },
                tFlow: flowmap.uniform
            }
        });

        const mesh = new ogl.Mesh(gl, { geometry, program });
        window.addEventListener("resize", resize, false);
        resize();

        // Mouse Velocity Logic
        let lastTime;
        const lastMouse = new ogl.Vec2();
        function updateMouse(e) {
            const x = e.pageX || (e.changedTouches && e.changedTouches[0].pageX);
            const y = e.pageY || (e.changedTouches && e.changedTouches[0].pageY);
            
            mouse.set(x / window.innerWidth, 1.0 - y / window.innerHeight);
            if (!lastTime) {
                lastTime = performance.now();
                lastMouse.set(x, y);
            }
            const deltaX = x - lastMouse.x;
            const deltaY = y - lastMouse.y;
            lastMouse.set(x, y);
            const time = performance.now();
            const delta = Math.max(10.4, time - lastTime);
            lastTime = time;
            velocity.x = deltaX / delta;
            velocity.y = deltaY / delta;
            velocity.needsUpdate = true;
        }

        window.addEventListener("mousemove", updateMouse);
        window.addEventListener("touchstart", updateMouse);
        window.addEventListener("touchmove", updateMouse);

        function update(t) {
            requestAnimationFrame(update);
            if (!velocity.needsUpdate) {
                mouse.set(-1);
                velocity.set(0);
            }
            velocity.needsUpdate = false;
            flowmap.aspect = window.innerWidth / window.innerHeight;
            flowmap.mouse.copy(mouse);
            flowmap.velocity.lerp(velocity, velocity.len ? 0.15 : 0.1);
            flowmap.update();
            program.uniforms.uTime.value = t * 0.01;
            renderer.render({ scene: mesh });
        }
        requestAnimationFrame(update);

        // --- 2. GSAP & MAGNETIC LOGIC ---
        const titles = document.querySelectorAll('[class*="section"] h4, [class*="section"] h5, .footer-title');
        titles.forEach(title => {
            const letters = title.querySelectorAll('.letter');
            if (letters.length > 0) {
                gsap.from(letters, {
                    scrollTrigger: {
                        trigger: title,
                        start: "top 90%",
                        toggleActions: "play none none reverse"
                    },
                    opacity: 0,
                    y: 40,
                    rotateX: -90,
                    stagger: 0.03,
                    duration: 0.8,
                    ease: "back.out(1.7)"
                });
            }
        });

        const magneticLinks = document.querySelectorAll('.magnetic-link');
        magneticLinks.forEach(link => {
            link.addEventListener('mousemove', (e) => {
                const rect = link.getBoundingClientRect();
                const x = e.clientX - rect.left - rect.width / 2;
                const y = e.clientY - rect.top - rect.height / 2;
                gsap.to(link, { x: x * 0.3, y: y * 0.3, duration: 0.3 });
            });
            link.addEventListener('mouseleave', () => {
                gsap.to(link, { x: 0, y: 0, duration: 0.5, ease: "elastic.out(1, 0.3)" });
            });
        });
    });
</script>

<div class="vhs-crt-overlay"></div>

<main>
    <div bind:this={canvasContainer} class="ogl-canvas-container"></div>

    <div class="landing-section">
        <LandingPageSection />
    </div>

    <div class="my-work-section-fake-gapper"></div>
    
    <section id="work-intro" class="my-work-section-title">
        <h4>
            {#each splitText("MY WORK") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h4>
    </section>

    <section id="project-1" class="my-work-section-first-project">
        <h5>
            {#each splitText("BATS!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h5>
    </section>
    
    <section id="project-2" class="my-work-section-second-project">
        <h5>
            {#each splitText("HEY JOH!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h5>
    </section>

    <section id="project-3" class="my-work-section-third-project">
        <h5>
            {#each splitText("DIKKE W!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h5>
    </section>

    <section class="about-me-section">
        <div class="deck-wrapper">
            <a href="#project-1" class="peek-card p-red"><span class="peek-label">BATS</span></a>
            <a href="#project-2" class="peek-card p-blue"><span class="peek-label">JOH</span></a>
            <a href="#project-3" class="peek-card p-yellow"><span class="peek-label">DIKKE</span></a>

            <div class="main-about-card">
                <div class="about-content">
                    <h5 class="about-title">
                        {#each splitText("ABOUT ME") as char}
                            <span class="letter">{char === " " ? "\u00A0" : char}</span>
                        {/each}
                    </h5>
                    <div class="about-details">
                        <p>Creative Developer & Interaction Designer</p>
                        <div class="skills-marquee">
                            <div class="marquee-content">
                                <span>SVELTEKIT • GSAP • OGL • MOTION • UI/UX • </span>
                                <span>SVELTEKIT • GSAP • OGL • MOTION • UI/UX • </span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <section class="contact-section">
        <div class="footer-container">
            <h5 class="footer-title">
                {#each splitText("LET'S WORK") as char}
                    <span class="letter">{char === " " ? "\u00A0" : char}</span>
                {/each}
            </h5>
            <div class="socials">
                <a href="mailto:hallo@site.nl" class="magnetic-link">EMAIL</a>
                <a href="https://linkedin.com" class="magnetic-link">LINKEDIN</a>
            </div>
        </div>
    </section>
</main>

<style>
    :global(html) {
        scroll-behavior: smooth;
        background-color: #000;
    }

    .ogl-canvas-container {
        position: fixed;
        inset: 0;
        z-index: 100; /* Voorop de pagina */
        pointer-events: none;
        mix-blend-mode: overlay; /* Blends de flowmap met de tekst erachter */
        opacity: 0.4;
    }

    .vhs-crt-overlay {
        position: fixed;
        inset: 0;
        z-index: 200;
        pointer-events: none;
        background: repeating-linear-gradient(0deg, rgba(0,0,0,0.03) 0px, rgba(0,0,0,0.03) 1px, transparent 1px, transparent 2px);
        box-shadow: inset 0 0 10vw rgba(0,0,0,0.5);
    }
    
    main {
        --fake-gapper-height: 200vh;
        --my-work-section-amount: 11; 
        --my-work-section-height: 100vh;
        --landing-section-height: 100vh;
        overflow-x: clip;
        position: relative;
        height: calc(var(--fake-gapper-height) + var(--landing-section-height) + (var(--my-work-section-amount) * var(--my-work-section-height)));
        
        /* CRT Ronde Hoeken Look */
        border: 1.5vw solid #111;
        border-radius: 4vw;
        margin: 1vw;
    }

    .letter { display: inline-block; perspective: 1000px; }

    @layer section-styles {
        [class*="section"] {
            display: flex;
            justify-content: center;
            align-items: center;
            position: sticky;
            top: 0;
            width: 100%;
            height: 100vh;
        }

        .landing-section {
            z-index: 10;
            animation: remove-section linear forwards, kill-h2 linear forwards;
            animation-timeline: scroll(root);
            animation-range: 450vh 550vh, 600vh 610vh;
        }

        .landing-section :global(h2) {
            position: absolute;
            color: #ff007f;
            font-size: var(--ultra-big-font-size); 
            text-transform: uppercase;
            animation: animation-move-to-center-and-scale linear forwards;
            animation-timeline: scroll(root);
            animation-range: 75vh var(--fake-gapper-height);
        }

        .my-work-section-fake-gapper { height: var(--fake-gapper-height); z-index: 5; position: sticky; top: 0; }
        
        .my-work-section-title { background-color: #333; z-index: 20; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 650vh 750vh; }
        .my-work-section-first-project { background-color: red; z-index: 30; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 640vh 750vh; }
        .my-work-section-second-project { background-color: blue; z-index: 35; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 630vh 750vh; }
        .my-work-section-third-project { background-color: yellow; z-index: 40; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 620vh 750vh; }

        .about-me-section { background-color: transparent; z-index: 45; animation: zoom-out-container linear forwards; animation-timeline: scroll(root); animation-range: 750vh 900vh; }

        .main-about-card {
            width: 100%; height: 100%; background-color: #3355ff;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
            z-index: 10; animation: round-card linear forwards; animation-timeline: scroll(root); animation-range: 750vh 900vh;
        }

        .peek-card {
            position: absolute; inset: 0; border-radius: 60px; text-decoration: none;
            display: flex; justify-content: center; align-items: center;
            animation: peek-scatter linear forwards; animation-timeline: scroll(root); animation-range: 920vh 1100vh;
        }

        .peek-label { color: white; font-weight: bold; opacity: 0; transition: 0.3s; }
        .peek-card:hover .peek-label { opacity: 1; }
        .peek-card:hover { transform: scale(1.05) !important; z-index: 15; }

        .p-red { background: red; z-index: 7; animation-name: peek-bl; }
        .p-blue { background: blue; z-index: 8; animation-name: peek-left; }
        .p-yellow { background: yellow; z-index: 9; animation-name: peek-tr; }

        .contact-section { z-index: 50; background-color: #000; }
        .socials { display: flex; gap: 40px; margin-top: 50px; }
        .socials a { color: white; text-decoration: none; font-size: 1.5rem; display: inline-block; }

        .skills-marquee { width: 100%; overflow: hidden; background: rgba(0,0,0,0.2); padding: 10px 0; margin-top: 30px; }
        .marquee-content { display: flex; white-space: nowrap; animation: marquee 20s linear infinite; gap: 20px; }
    }

    @keyframes animation-move-to-center-and-scale {
        0% { transform: translate(0, 0) scale(1); top: 0%; left: 0%; }
        30% { transform: translate(-50%, -50%) scale(1); top: 50%; left: 50%; }
        100% { transform: translate(-50%, -50%) scale(100); top: 50%; left: 50%; }
    }
    @keyframes card-shuffle-out { to { transform: translateX(110%) rotate(5deg); opacity: 0; } }
    @keyframes zoom-out-container { to { transform: scale(0.8); } }
    @keyframes round-card { to { border-radius: 60px; } }
    @keyframes peek-bl { to { transform: translate(-18%, 15%) rotate(-15deg); } }
    @keyframes peek-left { to { transform: translate(-22%, 0%) rotate(-8deg); } }
    @keyframes peek-tr { to { transform: translate(18%, -15%) rotate(15deg); } }
    @keyframes marquee { 0% { transform: translateX(0); } 100% { transform: translateX(-50%); } }
    @keyframes kill-h2 { to { visibility: hidden; display: none; } }
    @keyframes remove-section { to { opacity: 0; } }

    h4, h5 {
        color: white;
        font-size: var(--ultra-big-font-size);
        text-transform: uppercase;
        font-family: var(--extra-expanded-font);
        margin: 0;
    }
</style>