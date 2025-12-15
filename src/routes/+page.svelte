<script>
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/ScrollTrigger';
    import { LandingPageSection } from '$lib';

    $effect(() => {
        gsap.registerPlugin(ScrollTrigger);
        
        // Animation for FIRST JustWingItText (goes UP)
        gsap.to(".just-wing-it-box-variation-1", {
            scrollTrigger: {
                trigger: "main",
                start: "top top",
                end: "top+=150 top",
                scrub: 0.5,
            },
            y: -100,
            opacity: 0,
            duration: 0.8
        });
        
        // Animation for SECOND JustWingItText (goes DOWN)
        gsap.to(".just-wing-it-box-variation-2", {
            scrollTrigger: {
                trigger: "main",
                start: "top top",
                end: "top+=150 top",
                scrub: 0.5,
            },
            y: 100,
            opacity: 0,
            duration: 0.8
        });
        
        // NEW: Animation for h3 - disappears into the title
        gsap.to("h3", {
            scrollTrigger: {
                trigger: "main",
                start: "top top",
                end: "top+=250 top",
                scrub: 0.7,
            },
            y: -60, // Moves up
            opacity: 0, // Fades out
            duration: 1.2,
            ease: "power2.inOut"
        });
    });
</script>

<main>
    <LandingPageSection />
</main>
