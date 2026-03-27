<script>
    import { LandingPageSection } from '$lib'
</script>

<main>
    <div class="landing-section">
        <LandingPageSection />
    </div>

    <div class="my-work-section-fake-gapper"></div>
    
    <section class="my-work-section-title">
        <h4>MY WORK</h4>
    </section>

    <section class="my-work-section-first-project">
        <h5>BATS!</h5>
    </section>
    
    <section class="my-work-section-second-project">
        <h5>HEY JOH!</h5>
    </section>
 
    <section class="my-work-section-third-project">
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
        
        [class*="section"]{
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
            /*
            animation: zoom-out-and-round linear forwards;
            animation-timeline: scroll(root);
             Animates during the 5th and 6th "viewports" of scroll 
            animation-range: 600vh 700vh;*/
        }

    }
/*----------------------------------- TYPOGRAPHY --------------------------------------*/
    h4, h5 {
        color: var(--color-neutral-2000);
        font-size: var(--ultra-big-font-size);
        text-transform: uppercase;
        font-family: var(--extra-expanded-font);
    }

/*----------------------------------- ANIMATIONS --------------------------------------*/
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

        @keyframes zoom-out-and-round {
            0% { transform: scale(1); border-radius: 0px; }
            50%, 100% { transform: scale(0.8); border-radius: 40px; }
        }

/*----------------------------------- Supports for animation timeline, If it doesn't support this, make sure that the fake gapper is removed so scroll doesn't take too long --------------------------------------*/

      @supports not (animation-timeline: scroll(root)) {
        .my-work-section-fake-gapper {
            display: none;
        }

        main {
            height: auto;
        }

        .landing-section :global(h2) {
            animation: none !important;
        }
    }

</style>