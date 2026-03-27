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

    <section class="about-me-section">
        <h5>ABOUT ME</h5>
    </section>
</main>

<style>
    :root {
        --fake-gapper-height: 250vh;
        --my-work-and-about-section-amount: 5;
        --my-work-section-height: 100vh;
        --landing-section-height: 100vh;
    }

    main {
        overflow-x: clip;
        position: relative;
        /* Total height calculation */
        height: calc(var(--fake-gapper-height) + var(--landing-section-height) + (var(--my-work-and-about-section-amount) * var(--my-work-section-height)));
    }

/*----------------------------------- Section styling --------------------------------------*/
    @layer section-styles {
        
        [class*="section"] {
            display: flex;
            justify-content: center;
            align-items: center;
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
            
            /* Animation */
            animation: remove-section linear forwards;
            animation-timeline: scroll(root);
            animation-range: 500vh 600vh; /* Adjust this range to control when the landing section fades out */
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
            animation-range: 75vh var(--fake-gapper-height);
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

            /* Animation */
            animation: card-shuffle-out linear forwards;
            animation-timeline: scroll(root);
            /* Starts at the end of the 3rd project (approx 650vh total) */
            animation-range: 680vh 750vh;
        }

        .my-work-section-first-project {
            background-color: red;
            z-index: 30;

            /* Animation */
            animation: card-shuffle-out linear forwards;
            animation-timeline: scroll(root);
            /* Starts at the end of the 3rd project (approx 650vh total) */
            animation-range: 670vh 750vh;
        }

        .my-work-section-second-project {
            background-color: blue;
            z-index: 35;

            /* Animation */
            animation: card-shuffle-out linear forwards;
            animation-timeline: scroll(root);
            /* Starts at the end of the 3rd project (approx 650vh total) */
            animation-range: 660vh 750vh;
        }

        .my-work-section-third-project {
            background-color: yellow;
            z-index: 40;
            /* Trigger the card shuffle when the About Me section 
               is scrolling into view (at the end of the work sections) 
            */
            animation: card-shuffle-out linear forwards;
            animation-timeline: scroll(root);
            /* Starts at the end of the 3rd project (approx 650vh total) */
            animation-range: 650vh 750vh;
        }

        .about-me-section {
            background-color: #3355ff;
            z-index: 45; /* Sits slightly higher than the work sections initially */
            animation: zoom-out-and-round linear forwards;
            animation-timeline: scroll(root);
            /* Starts at the end of the 3rd project (approx 650vh total) */
            animation-range: 650vh 750vh;
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
    @keyframes animation-move-to-center-and-scale {
        0% { 
            transform: translate(0, 0) scale(1); 
            top: 0%; 
            left: 0%; 
        }
        30% { 
            transform: translate(-50%, -50%) scale(1); 
            top: 50%; 
            left: 50%; 
        }
        50% { 
            transform: translate(-50%, -50%) scale(5); 
            top: 50%; 
            left: 50%; 
        }
        100% { 
            transform: translate(-50%, -50%) scale(100); 
            top: 50%; 
            left: 50%; 
        }
    }

    @keyframes card-shuffle-out {
        0% {
            transform: translateX(0) scale(1);
            border-radius: 0px;
        }
        100% {
            transform: translateX(120%) rotate(5deg) scale(1);
            border-radius: 0px;
        }
    }

    @keyframes remove-section {
        0% { 
            opacity: 1; 
        }
        100% { 
            opacity: 0; 
        }
    }

    @keyframes zoom-out-and-round {
        0% { 
            transform: scale(1); 
            border-radius: 0px; 
        }
        100% { 
            transform: scale(0.8); 
            border-radius: 60px; 
        }
    }
/*----------------------------------- Supports --------------------------------------*/
    @supports not (animation-timeline: scroll(root)) {
        .my-work-section-fake-gapper { 
            display: none; 
        }
        main { 
            height: auto; 
        }
        [class*="section"]  { 
            animation: none !important; 
        }
    }
</style>