<script>
    import { LandingPageSection } from '$lib';
    import { onMount } from 'svelte';
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/dist/ScrollTrigger';
    import { ScrollToPlugin } from 'gsap/dist/ScrollToPlugin';
    import * as ogl from 'ogl'; 

    gsap.registerPlugin(ScrollTrigger, ScrollToPlugin);

    let canvasContainer;

    // --- SNAP LOGIC ---
    const goToNextSection = () => {
        const vh = window.innerHeight;
        const currentScroll = window.scrollY;
        
        // Steps: Landing (0), Work Intro (3.5vh), Proj 1 (4.5vh), Proj 2 (5.5vh), Proj 3 (6.5vh), About (7.5vh), Contact (8.5vh)
        const stops = [0, 3.5 * vh, 4.5 * vh, 5.5 * vh, 6.5 * vh, 7.5 * vh, 8.5 * vh];
        
        const nextStop = stops.find(stop => stop > currentScroll + 50);

        if (nextStop !== undefined) {
            gsap.to(window, {
                scrollTo: nextStop,
                duration: 0.2, 
                ease: "power3.out" 
            });
        } else {
            gsap.to(window, { scrollTo: 0, duration: 1, ease: "power3.inOut" });
        }
    };

    onMount(() => {
        // --- 1. OGL FLOWMAP LOGIC (Fixed Viewport) ---
        const vertex = `attribute vec2 uv; attribute vec2 position; varying vec2 vUv; void main() { vUv = uv; gl_Position = vec4(position, 0, 1); }`;
        const fragment = `
            precision highp float;
            uniform sampler2D tWater; uniform sampler2D tFlow; varying vec2 vUv; uniform vec4 res;
            void main() {
                vec3 flow = texture2D(tFlow, vUv).rgb;
                vec2 uv = .5 * gl_FragCoord.xy / res.xy;
                vec2 myUV = (uv - vec2(0.5)) * res.zw + vec2(0.5);
                myUV -= flow.xy * 0.15;
                gl_FragColor = vec4(texture2D(tWater, myUV).r, texture2D(tWater, myUV - flow.xy * 0.12).g, texture2D(tWater, myUV - flow.xy * 0.10).b, 1.0);
            }
        `;
        const renderer = new ogl.Renderer({ dpr: 2, alpha: true });
        const gl = renderer.gl;
        canvasContainer.appendChild(gl.canvas);
        const mouse = new ogl.Vec2(-1);
        const velocity = new ogl.Vec2();
        const flowmap = new ogl.Flowmap(gl, { falloff: 0.3, dissipation: 0.92 });
        const geometry = new ogl.Geometry(gl, {
            position: { size: 2, data: new Float32Array([-1, -1, 3, -1, -1, 3]) },
            uv: { size: 2, data: new Float32Array([0, 0, 2, 0, 0, 2]) }
        });
        const texture = new ogl.Texture(gl, { minFilter: gl.LINEAR });
        const img = new Image();
        img.crossOrigin = "Anonymous";
        img.src = "https://robindelaporte.fr/codepen/bg3.jpg";
        img.onload = () => (texture.image = img);
        const program = new ogl.Program(gl, {
            vertex, fragment,
            uniforms: { uTime: { value: 0 }, tWater: { value: texture }, res: { value: new ogl.Vec4(0) }, tFlow: flowmap.uniform }
        });
        const mesh = new ogl.Mesh(gl, { geometry, program });
        const resize = () => {
            renderer.setSize(window.innerWidth, window.innerHeight);
            program.uniforms.res.value.set(window.innerWidth, window.innerHeight, 1, 1);
        };
        window.addEventListener("resize", resize); resize();
        window.addEventListener("mousemove", (e) => {
            mouse.set(e.clientX / window.innerWidth, 1.0 - (e.clientY / window.innerHeight));
            velocity.set(e.movementX / 10, e.movementY / 10); 
            velocity.needsUpdate = true;
        });
        const update = (t) => {
            requestAnimationFrame(update);
            if (!velocity.needsUpdate) { mouse.set(-1); velocity.set(0); }
            velocity.needsUpdate = false;
            flowmap.aspect = window.innerWidth / window.innerHeight;
            flowmap.mouse.copy(mouse);
            flowmap.velocity.lerp(velocity, 0.2);
            flowmap.update();
            renderer.render({ scene: mesh });
        };
        requestAnimationFrame(update);


        // --- 2. TEXT ANIMATIONS (The New Part) ---
        const sections = document.querySelectorAll('section');
        sections.forEach(section => {
            const letters = section.querySelectorAll('.letter');
            if (letters.length > 0) {
                gsap.from(letters, {
                    scrollTrigger: {
                        trigger: section,
                        start: "top 80%", // Start als de sectie bijna in beeld is
                        toggleActions: "play none none reverse"
                    },
                    y: 60,
                    opacity: 0,
                    rotateX: -90, // 3D kanteling
                    stagger: 0.03, // Vertraging per letter
                    duration: 0.8,
                    ease: "power4.out"
                });
            }
        });

        // --- 3. BUTTON ANIMATION ---
        gsap.to(".nav-pilot-btn", {
            y: -12,
            duration: 1.2,
            repeat: -1,
            yoyo: true,
            ease: "sine.inOut"
        });
    });

    const splitText = (text) => text.split("");

    let trailPoints = Array(25).fill({x: 0, y: 0}); // Aantal punten in de lijn
    
    function moveTrail() {
        // Update de staart: elk punt volgt het punt ervoor met een vertraging
        for (let i = trailPoints.length - 1; i > 0; i--) {
            trailPoints[i].x += (trailPoints[i-1].x - trailPoints[i].x) * 0.3;
            trailPoints[i].y += (trailPoints[i-1].y - trailPoints[i].y) * 0.3;
        }
        trailPoints = trailPoints; // Svelte reactivity trigger
        requestAnimationFrame(moveTrail);
    }

    onMount(() => {
        window.addEventListener('mousemove', (e) => {
            trailPoints[0] = { x: e.clientX, y: e.clientY };
        });
        moveTrail();
    });

    // Helper om de punten om te zetten naar een SVG string
    $: pathData = `M ${trailPoints.map(p => `${p.x},${p.y}`).join(' L ')}`;
</script>

<button class="nav-pilot-btn" on:click={goToNextSection}>
    <span class="btn-text">NEXT PHASE</span>
    <div class="btn-bg"></div>
</button>

<main>
    <div bind:this={canvasContainer} class="ogl-canvas-container"></div>

    <div class="landing-section">
        <LandingPageSection />
    </div>

    <div class="my-work-section-fake-gapper"></div>
    
    <section class="my-work-section-title">
        <h4>
            {#each splitText("MY WORK") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h4>
    </section>

    <section class="my-work-section-first-project">
        <h5>
            {#each splitText("BATS!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h5>
    </section>
    
    <section class="my-work-section-second-project">
        <h5>
            {#each splitText("HEY JOH!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h5>
    </section>
 
    <section class="my-work-section-third-project">
        <h5>
            {#each splitText("DIKKE W!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </h5>
    </section>

    <section class="about-me-section">
        <a href="#contact" class="about-clickable-content">
            <h5>
                {#each splitText("ABOUT ME") as char}
                    <span class="letter">{char === " " ? "\u00A0" : char}</span>
                {/each}
            </h5>
            <p class="click-hint">CLICK TO CONNECT</p>
        </a>
    </section>

    <section id="contact" class="contact-section">
        <div class="contact-wrapper">
            <h5>
                {#each splitText("LET'S WORK") as char}
                    <span class="letter">{char === " " ? "\u00A0" : char}</span>
                {/each}
            </h5>
            <div class="contact-links">
                <a href="mailto:hallo@example.com">EMAIL</a>
                <a href="https://linkedin.com">LINKEDIN</a>
            </div>
        </div>
    </section>
</main>

    <svg class="mouse-trail-svg">
        <path d={pathData} class="trail-path" />
    </svg>

<style>
.mouse-trail-svg {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        z-index: 15000;
    }

    .trail-path {
        fill: none;
        stroke: #adff2f; 
        /* --- Dikte aangepast naar 60px --- */
        stroke-width: 60; 
        stroke-linecap: round;
        stroke-linejoin: round;
        
        /* Lager gezet zodat de dikke lijn de tekst niet volledig blokkeert */
        stroke-opacity: 1;
        
        /* Maakt de kleuren intenser waar ze de tekst raken */
        mix-blend-mode: screen;

        /* --- Gigantische Neon Glow --- */
        filter: 
            blur(1px)
            drop-shadow(0 0 20px rgba(173, 255, 47, 0.9)) 
            drop-shadow(0 0 50px rgba(173, 255, 47, 0.6))
            drop-shadow(0 0 100px rgba(173, 255, 47, 0.3));
    }
    :global(main){
          mix-blend-mode: luminosity;
    }
    :global(html) {
        scroll-behavior: smooth;
        background-color: #000;
    }

    .ogl-canvas-container {
        position: fixed;
        inset: 0;
        z-index: 10000; 
        pointer-events: none;
        mix-blend-mode: difference; 
    }

    /* NAVIGATION BUTTON - BIG & CENTERED */
    .nav-pilot-btn {
        position: fixed;
        bottom: 5vh;
        left: 50%;
        transform: translateX(-50%);
        z-index: 20000;
        background: none;
        border: none;
        cursor: pointer;
        padding: 24px 64px; /* Significantly larger padding */
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .btn-text {
        color: #adff2f;
        font-family: var(--extra-expanded-font);
        font-size: 2rem; /* Much larger font */
        font-weight: 900;
        letter-spacing: 0.5rem; /* Wider spacing */
        position: relative;
        z-index: 2;
        white-space: nowrap;
    }

    .btn-bg {
        position: absolute;
        inset: 0;
        border: 2px solid #adff2f; /* Thicker border */
        background: rgba(173, 255, 47, 0.05);
        backdrop-filter: blur(10px);
        transform: skewX(-12deg);
        z-index: 1;
        transition: 0.3s cubic-bezier(0.23, 1, 0.32, 1);
    }

    .nav-pilot-btn:hover .btn-bg {
        background: #adff2f;
        transform: skewX(-12deg) scale(1.1);
    }
    
    .nav-pilot-btn:hover .btn-text {
        color: #000;
    }
    
    main {
        --fake-gapper-height: 250vh;
        --my-work-and-about-section-amount: 6;
        --my-work-section-height: 100vh;
        --landing-section-height: 100vh;
        overflow-x: clip;
        position: relative;
        height: calc(var(--fake-gapper-height) + var(--landing-section-height) + (var(--my-work-and-about-section-amount) * var(--my-work-section-height)));
    }

    @layer section-styles {
        [class*="section"] {
            display: flex; justify-content: center; align-items: center;
            position: sticky; top: 0; width: 100%; height: var(--my-work-section-height);
        }

        .landing-section {
            z-index: 10; position: relative; overflow: hidden; 
            animation: remove-section linear forwards; animation-timeline: scroll(root); animation-range: 500vh 600vh;
        }

        .landing-section :global(h2) {
            position: absolute; margin: 0; white-space: nowrap; color: var(--color-neutral-2000); 
            font-size: var(--ultra-big-font-size); font-family: var(--extra-expanded-font); text-transform: uppercase;
            animation: animation-move-to-center-and-scale linear forwards;
            animation-timeline: scroll(root); animation-range: 75vh var(--fake-gapper-height);
        }

        .my-work-section-fake-gapper { height: var(--fake-gapper-height); top: 0; z-index: 5; background: transparent; pointer-events: none; } 
        
        .my-work-section-title { background-color: red; color: #000; z-index: 20; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 680vh 750vh; }
        .my-work-section-first-project { background-color: red; color: #0051ff; z-index: 30; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 670vh 750vh; }
        .my-work-section-second-project { background-color: blue; z-index: 35; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 660vh 750vh; }
        .my-work-section-third-project { background-color: #ff00bf; z-index: 40; animation: card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 650vh 750vh; }

        .about-me-section {
            background-color: #3355ff; z-index: 45;
            animation: zoom-out-and-round linear forwards;
            animation-timeline: scroll(root); animation-range: 750vh 900vh;
        }

        .about-clickable-content { text-decoration: none; display: flex; flex-direction: column; align-items: center; }
        .click-hint { color: white; font-family: var(--extra-expanded-font); font-size: 1.2rem; margin-top: 1rem; opacity: 0.7; }
        .contact-section { background-color: #000; z-index: 50; }
        .contact-wrapper { text-align: center; }
        .contact-links { display: flex; gap: 2rem; justify-content: center; margin-top: 2rem; }
        .contact-links a { color: white; text-decoration: none; font-size: 1.5rem; }
    }

    h4, h5 { color: #adff2f; font-size: var(--ultra-big-font-size); text-transform: uppercase; font-family: var(--extra-expanded-font); margin: 0; }

    @keyframes animation-move-to-center-and-scale {
        0% { transform: translate(0, 0) scale(1); top: 0%; left: 0%; }
        30% { transform: translate(-50%, -50%) scale(1); top: 50%; left: 50%; }
        50% { transform: translate(-50%, -50%) scale(5); top: 50%; left: 50%; }
        100% { transform: translate(-50%, -50%) scale(100); top: 50%; left: 50%; }
    }
    @keyframes card-shuffle-out { 0% { transform: translateX(0); } 100% { transform: translateX(120%) rotate(5deg); } }
    @keyframes remove-section { 0% { opacity: 1; } 100% { opacity: 0; } }
    @keyframes zoom-out-and-round { 0% { transform: scale(1); } 100% { transform: scale(0.8); border-radius: 60px; } }
</style>