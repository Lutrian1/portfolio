<script>
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/ScrollTrigger';
    import { LandingPageSection } from '$lib';

    $effect(() => {
        gsap.registerPlugin(ScrollTrigger);
        
        // All animations in sequence
        const tl = gsap.timeline({
            scrollTrigger: {
                trigger: "main",
                start: "top top",
                end: "top+=900 top",
                scrub: 1.3,
                markers: true,
                pin: true
            }
        });
        
        // 1. Wing texts disappear together
        tl.to(".just-wing-it-box-variation-1", {
            y: -100,
            opacity: 0,
            duration: 0.5
        });
        
        tl.to(".just-wing-it-box-variation-2", {
            y: 100,
            opacity: 0,
            duration: 0.5
        }, "<"); // Same time
        
        // 2. h3 - only moves up and fades (NO scale)
        tl.to("h3", {
            y: -60,
            opacity: 0,
            duration: 0.6
        }, ">");
        
        // 3. h2 moves to center
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
            
            // 4. h2 zooms out
            tl.to(h2, {
                scale: 100,
                duration: 1.4
            }, ">");
        }
    });
</script>

<main>
    <LandingPageSection />
</main>

<style>
    main{
        overflow-x: clip;
    }
</style>