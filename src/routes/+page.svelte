<script>
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/ScrollTrigger';
    import { LandingPageSection } from '$lib';
    
    $effect(() => {
        gsap.registerPlugin(ScrollTrigger);
        
        const tl = gsap.timeline({
            scrollTrigger: {
                trigger: "main",
                start: "top top",
                end: "+=1200", // Increased for smooth transition
                scrub: 1.3,
            }
        });
        
        // Your existing animations...
        tl.to(".just-wing-it-box-variation-1", {
            y: -100,
            opacity: 0,
            duration: 0.5
        });
        
        tl.to(".just-wing-it-box-variation-2", {
            y: 100,
            opacity: 0,
            duration: 0.5
        }, "<");
        
        tl.to("h3", {
            y: -60,
            opacity: 0,
            duration: 0.6
        }, ">");
        
        const h2 = document.querySelector('h2');
        if (h2) {
            gsap.set(h2, { transformOrigin: "center center" });
            
            const centerX = window.innerWidth / 2;
            const centerY = window.innerHeight / 2;
            const rect = h2.getBoundingClientRect();
            
            tl.to(h2, {
                x: centerX - (rect.left + rect.width / 2),
                y: centerY - (rect.top + rect.height / 2),
                duration: 0.9
            }, ">");
            
            tl.to(h2, {
                scale: 100,
                duration: 1.4
            }, ">");
        }
        
        // Slide up the landing section and slide in the next section
        tl.to(".landing-section", {
            y: "-100vh",
            duration: 1
        }, ">");
        
        tl.to(".next-section", {
            y: "-100vh",
            duration: 1
        }, "<"); // Same time
    });
</script>

<main>
    <div class="landing-section">
        <LandingPageSection />
    </div>
    
    <section class="next-section">
        <h4>MY WORK</h4>
    </section>
</main>

<style>
    main {
        overflow-x: clip;
        position: relative;
    }
    
    .landing-section {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        z-index: 20;
    }
    
    .next-section {
        position: fixed;
        top: 100vh; /* Start just below viewport */
        left: 0;
        width: 100%;
        height: 100vh;
        background-color: var(--color-neutral-300);
        z-index: 10;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    
    h4 {
        color: white;
        font-size: 20rem;
        font-size: var(--ultra-big-font-size);
        text-transform: uppercase;
    }
</style>