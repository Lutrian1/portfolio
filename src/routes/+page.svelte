<script>
    import { gsap } from 'gsap'
    import { ScrollTrigger } from 'gsap/ScrollTrigger'
    import { LandingPageSection } from '$lib'

    // $effect(() => {
    //     // -------------------------------------------------------------------- //
    //     // ---------------------- 1. Alle variabelen -------------------------- //
    //     // -------------------------------------------------------------------- //

    //     const root = document.documentElement
    //     const main = document.querySelector('main')
    //     const h2 = document.querySelector('h2')

    //     // -------------------------------------------------------------------- //
    //     // ------------------------- 2. Code Logic ---------------------------- //
    //     // -------------------------------------------------------------------- //

    //     gsap.registerPlugin(ScrollTrigger)

    //     // Enable JS and Remove JS-disabled class that is standardly loaded
    //     root.classList.remove('js-disabled')
    //     root.classList.add('js-enabled')

    //     // Set main height to the total height of all sections so there are 4. 4 * 100 (each being 100vh in height)
    //     main.style.height = '400vh'

    //     const timeline = gsap.timeline({
    //         scrollTrigger: {
    //             trigger: 'main',
    //             start: 'top top',
    //             end: '+=2000',
    //             scrub: 1.6 
    //         }
    //     })

    //     timeline.to('.just-wing-it-box-variation-1', {
    //         y: -100,
    //         opacity: 0,
    //         duration: 1 
    //     })

    //     timeline.to('.just-wing-it-box-variation-2', {
    //         y: 100,
    //         opacity: 0,
    //         duration: 1
    //     }, '<')

    //     timeline.to('h3', {
    //         y: -60,
    //         opacity: 0,
    //         duration: 1.2
    //     }, '>')

    //     if (h2) {
    //         const h2Rect = h2.getBoundingClientRect()
    //         const windowWidth = window.innerWidth
    //         const windowHeight = window.innerHeight

    //         timeline.to(h2, {
    //             x: windowWidth / 2 - (h2Rect.left + h2Rect.width / 2),
    //             y: windowHeight / 2 - (h2Rect.top + h2Rect.height / 2),
    //             duration: 2, 
    //             ease: 'expo.inOut'
    //         })

    //         timeline.to(h2, {
    //             scale: 100,
    //             duration: 2, 
    //             ease: 'expo.inOut'
    //         })
    //     }

    //     timeline.to('.landing-section', {
    //         y: '-100vh',
    //         duration: 2 
    //     }, '>')
        
    //     timeline.to('.my-work-section-title', {
    //         y: '-100vh',
    //         duration: 2
    //     }, '<')
        
    //     gsap.set('.my-work-section-first-project', { y: '100vh' })
        
    //     timeline.to('.my-work-section-first-project', {
    //         y: '0vh',
    //         duration: 2,
    //         ease: 'power2.out'
    //     }, '>')
        
    //     gsap.set('.my-work-section-second-project', { y: '100vh' })
        
    //     timeline.to('.my-work-section-first-project', {
    //         y: '-100vh',
    //         duration: 2
    //     }, '>')
        
    //     timeline.to('.my-work-section-second-project', {
    //         y: '0vh',
    //         duration: 2
    //     }, '<')
        
    //     gsap.set('.my-work-section-third-project', { y: '100vh' })
        
    //     timeline.to('.my-work-section-second-project', {
    //         y: '-100vh',
    //         duration: 2
    //     }, '>')
        
    //     timeline.to('.my-work-section-third-project', {
    //         y: '0vh',
    //         duration: 2
    //     }, '<')
        
    //     timeline.to('.my-work-section-third-project', {
    //         y: '-100vh',
    //         duration: 2
    //     }, '>')
    // })
</script>

<main>
    <div class="landing-section js-disabled">
        <LandingPageSection />
    </div>

    <div class="my-work-section-fake-gapper js-disabled"></div>
    
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
    :root{
        --fake-gapper-height: 250vh;
        --my-work-section-amount: 4; /* Change this to the amount of 'my-work' sections (except gapper) that there are on the page*/
        --my-work-section-height: 100vh;
        --landing-section-height: 100vh;
    }

    main {
        overflow-x: clip;
        position: relative;
        height: calc(var(--fake-gapper-height) + var(--landing-section-height) + (var(--my-work-section-amount) * var(--my-work-section-height))); /* 350vh (landing + fake gapper) + 100vh × 4 sections*/
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
            height: var(--my-work-section-height);
        }

        /* --- Styling for all seperate sections --- */
        .landing-section {
            top: 0;
            z-index: 10;
            position: relative; 
            overflow: hidden; 
        }

        .landing-section :global(h2) {
            position: absolute;
            margin: 0;
            white-space: nowrap;
            color: var(--color-neutral-2000); 
            font-size: var(--ultra-big-font-size); 
            font-family: var(--extra-expanded-font); 
            text-transform: uppercase;
            
            /* Animation */
            animation: animation-move-to-center-and-scale linear forwards;
            animation-timeline: scroll(root);
            animation-range: 75vh var(--fake-gapper-height); /* Animation starts at the first value, ends at the height of the fake-gapper */
        }

        /* Keyframes for the text animation */
        @keyframes animation-move-to-center-and-scale {
            0% {
                top: 0%; 
                left: 0%; 
                transform: translate(0, 0) scale(1);
            }
            30% {
                /* Moves to center */
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%) scale(1);
            }
            50% {
                /* Starts scaling up */
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%) scale(5);
            }
            100% {
                /* Final state - large scale and faded */
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%) scale(100);
            }
        }

        .my-work-section-fake-gapper {
            height: var(--fake-gapper-height);
            top: 0;
            z-index: 5;
            background: transparent;
            pointer-events: none;
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

        /* Disable CSS animations when JS is enabled */
        :global(.js-enabled) .landing-section :global(h2) {
            animation: none;
            position: fixed;
        }

        /* Hide the fake gapper when JS is enabled */
        :global(.js-enabled) .my-work-section-fake-gapper {
            display: none;
        }

        :global(.js-enabled) [class^="my-work-section"] {
            height: var(--my-work-section-height);
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

/*----------------------------------- Supports for animation timeline, If it doesn't support this, make sure that the fake gapper is removed so scroll doesn't take too long --------------------------------------*/

      @supports not (animation-timeline: scroll(root)) {
        .my-work-section-fake-gapper {
            display: none;
        }

        main {
            height: auto;
        }
    }

</style>