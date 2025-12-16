<script>
    import { gsap } from 'gsap';
    import { ScrollTrigger } from 'gsap/ScrollTrigger';
    import { LandingPageSection } from '$lib';

    $effect(() => {
        gsap.registerPlugin(ScrollTrigger);

        const root = document.documentElement;
        const main = document.querySelector('main');
        const scroll_height = parseInt(getComputedStyle(document.documentElement).getPropertyValue('--scroll-animation-height-for-GSAP'),10); 

        // Enable JS and Remove JS-disabled class that is standardly loaded.
        root.classList.remove('js-disabled');
        root.classList.add('js-enabled');

        // Set main height to the total height of all sections so there are 4. 4 * 100 (each being 100vh in height)
        main.style.height = '400vh';

        const tl = gsap.timeline({
            scrollTrigger: {
                trigger: 'main',
                start: 'top top',
                end: '+=2000',
                scrub: 1.3,
            }
        });

        tl.to('.just-wing-it-box-variation-1', {
            y: -100,
            opacity: 0,
            duration: 0.5
        });

        tl.to('.just-wing-it-box-variation-2', {
            y: 100,
            opacity: 0,
            duration: 0.5
        }, '<');

        tl.to('h3', {
            y: -60,
            opacity: 0,
            duration: 0.6
        }, '>');

        const h2 = document.querySelector('h2');
        if (h2) {
            const rect = h2.getBoundingClientRect();

            tl.to(h2, {
                x: window.innerWidth / 2 - (rect.left + rect.width / 2),
                y: window.innerHeight / 2 - (rect.top + rect.height / 2),
                duration: 0.9
            });

            tl.to(h2, {
                scale: 100,
                duration: 1.4
            });
        }

        tl.to('.landing-section', { y: '-100vh', duration: 1 });
        tl.to('.my-work-section-title', { y: '-100vh', duration: 1 }, '<');

        gsap.set('.my-work-section-first-project', { y: '100vh' });
        tl.to('.my-work-section-first-project', { y: '0vh', duration: 1 });

        gsap.set('.my-work-section-second-project', { y: '100vh' });
        tl.to('.my-work-section-first-project', { y: '-100vh', duration: 1 });
        tl.to('.my-work-section-second-project', { y: '0vh', duration: 1 }, '<');

        gsap.set('.my-work-section-third-project', { y: '100vh' });
        tl.to('.my-work-section-second-project', { y: '-100vh', duration: 1 });
        tl.to('.my-work-section-third-project', { y: '0vh', duration: 1 }, '<');
        tl.to('.my-work-section-third-project', { y: '-100vh', duration: 1 });
    });
</script>


<main>
    <div class="landing-section js-disabled">
        <LandingPageSection />
    </div>
    
    <section class="my-work-section-title js-disabled">
        <h4>MY WORK</h4>
    </section>

    <section class="my-work-section-first-project js-disabled">
        <h5>BATS!</h5>
    </section>
    
    <section class="my-work-section-second-project js-disabled">
        <h5>HEY JOH!</h5>
    </section>
 
    <section class="my-work-section-third-project js-disabled">
        <h5>DIKKE W!</h5>
    </section>
</main>

<style>

    main {
        overflow-x: clip;
        position: relative;
        height: auto;
    }

/*----------------------------------- Section styling --------------------------------------*/
    @layer section-styles {
        
        [class^="my-work-section"]{
            display: flex;
            justify-content: center;
            align-items: center;
        }

    /*----------------------------------- Section styling (JS DISABLED) --------------------------------------*/
        /* Styling when js is disabled */
        .js-disabled{
            position: sticky;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
        }

        /* --- Styling for all seperate sections --- */
        .landing-section {
            top: 0;
            z-index: 10;
        }
        
        .my-work-section-title {
            background-color: var(--color-neutral-300);
            z-index: 20;
        }

        .my-work-section-first-project{
            background-color: red;
            z-index: 30;
        }

        .my-work-section-second-project{
            background-color: blue;
            z-index: 35;
        }

        .my-work-section-third-project{
            background-color: yellow;
            z-index: 40;
        }

    /*----------------------------------- Section styling (JS ENABLED) --------------------------------------*/

        /* These styles will only apply when JS adds the js-enabled class */

        /* General section styling (when JS is Enabled) */
        :global(.js-enabled) .landing-section,
        :global(.js-enabled) [class^="my-work-section"] {
            position: fixed;
            width: 100%;
            left: 0;
        }

        :global(.js-enabled) [class^="my-work-section"] {
            height: 100vh;
            top: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* --- Styling for individual sections (when JS is Enabled) --- */
        :global(.js-enabled) .landing-section {
            top: 0;
            z-index: 10;
        }

        :global(.js-enabled) .my-work-section-title {
            background-color: var(--color-neutral-300);
            z-index: 20;
        }

        :global(.js-enabled) .my-work-section-first-project {
            background-color: red;
            z-index: 30;
        }

        :global(.js-enabled) .my-work-section-second-project {
            background-color: blue;
            z-index: 35;
        }

        :global(.js-enabled) .my-work-section-third-project {
            background-color: yellow;
            z-index: 40;
        }

    }
/*----------------------------------- TYPOGRAPHY --------------------------------------*/
    h4, h5 {
        color: var(--color-neutral-2000);
        font-size: var(--ultra-big-font-size);
        text-transform: uppercase;
        font-family: var(--extra-expanded-font);
    }
</style>