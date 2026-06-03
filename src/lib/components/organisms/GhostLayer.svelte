<script>
    const splitText = (text) => text.split("");
</script>

<div class="ghost-overlay" aria-hidden="true">
    
    <div class="ghost-section ghost-landing-section">
        <div class="ghost-landing-wrapper">
            <div class="ghost-article-box">
                <div class="ghost-h2">Web developer</div>
            </div>
        </div>
    </div>

    <div class="ghost-fake-gapper"></div>
    
    <div class="ghost-section ghost-work-title">
        <div class="h4-ghost ghost-text-1">
            {#each splitText("MY WORK") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </div>
    </div>

    <div class="ghost-section ghost-first-project">
        <div class="h5-ghost ghost-text-2">
            {#each splitText("BATS!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </div>
    </div>
    
    <div class="ghost-section ghost-second-project">
        <div class="h5-ghost ghost-text-3">
            {#each splitText("HEY JOH!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </div>
    </div>
 
    <div class="ghost-section ghost-third-project">
        <div class="h5-ghost">
            {#each splitText("DIKKE W!") as char}
                <span class="letter">{char === " " ? "\u00A0" : char}</span>
            {/each}
        </div>
    </div>

    <div class="ghost-section ghost-about-section">
        <div class="ghost-about-clickable">
            <div class="h5-ghost">
                {#each splitText("ABOUT ME") as char}
                    <span class="letter">{char === " " ? "\u00A0" : char}</span>
                {/each}
            </div>
            <p class="ghost-click-hint">CLICK TO CONNECT</p>
        </div>
    </div>

    <div class="ghost-section ghost-contact-section">
        <div class="ghost-contact-wrapper">
            <div class="h5-ghost">
                {#each splitText("LET'S WORK") as char}
                    <span class="letter">{char === " " ? "\u00A0" : char}</span>
                {/each}
            </div>
            <div class="ghost-contact-links">
                <span>EMAIL</span>
                <span>LINKEDIN</span>
            </div>
        </div>
    </div>
</div>

<style>
    /* ========================================================= */
    /* 1. GHOST BASE STYLING & NEON OUTLINES                     */
    /* ========================================================= */
    .ghost-overlay {
        --fake-gapper-height: 250vh;
        --my-work-and-about-section-amount: 6;
        --my-work-section-height: 100vh;
        --landing-section-height: 100vh;
        
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: calc(var(--fake-gapper-height) + var(--landing-section-height) + (var(--my-work-and-about-section-amount) * var(--my-work-section-height)));
        
        overflow-x: clip;
        z-index: 10001;
        pointer-events: none;
    }

    .letter { display: inline-block; }

    /* De Neon Stroke */
    .letter, .ghost-h2, .ghost-click-hint, .ghost-contact-links span {
        color: transparent !important;
        -webkit-text-stroke: 1.5px #adff2f;
        filter: drop-shadow(0 0 5px rgba(173, 255, 47, 0.6));
    }


    /* ========================================================= */
    /* 2. LANDING PAGE SECTION REPLICA                           */
    /* ========================================================= */
    .ghost-landing-section {
        z-index: 10; position: relative; overflow: hidden; 
        animation: ghost-remove-section linear forwards; 
        animation-timeline: scroll(root); 
        animation-range: 500vh 600vh;
    }

    .ghost-landing-wrapper {
        min-height: calc(100% - calc(var(--grid-gap) * 8));
        padding: calc(var(--grid-gap) * 4) 0;
        position: fixed; display: grid; grid-template-columns: var(--grid-columns);
        width: 100%; left: 0; pointer-events: none;
    }

    .ghost-article-box {
        position: relative; grid-column: 2 / var(--grid-colomn-amount);
        width: calc(100% + var(--grid-gap) * 2); left: calc(-1 * var(--grid-gap));
    }

    .ghost-h2 {
        font-family: var(--semi-condensed-font); text-transform: uppercase; font-weight: normal; margin: 0;
        text-align: center; position: absolute; height: 13.536vw; font-size: var(--ultra-big-font-size); white-space: nowrap;
        
        animation: ghost-move-to-center-and-scale linear forwards;
        animation-timeline: scroll(root); animation-range: 75vh var(--fake-gapper-height);

        @media (min-width: 768px) {
            text-align: left;
            text-indent: calc(var(--grid-gap) * 2);
        }
    }

    .ghost-fake-gapper { height: var(--fake-gapper-height); top: 0; z-index: 5; position: sticky; } 


    /* ========================================================= */
    /* 3. STICKY SECTIONS & CROSS-FADE LOGIC                     */
    /* ========================================================= */
    .ghost-section {
        display: flex; justify-content: center; align-items: center;
        position: sticky; top: 0; width: 100%; height: var(--my-work-section-height);
        background-color: transparent !important;
        perspective: 1000px;
    }

    .h4-ghost, .h5-ghost {
        font-size: var(--ultra-big-font-size); text-transform: uppercase; font-family: var(--extra-expanded-font); margin: 0;
    }

    /* --- DE OVERLAP FIX --- */
    /* Zodra de *volgende* kaart op 100vh (top) komt te staan, faden we deze uit */
    
    .ghost-text-1 {
        animation: ghost-fade-out linear forwards;
        animation-timeline: scroll(root);
        /* "MY WORK" verdwijnt exact wanneer "BATS!" (op 450vh) vastplakt */
        animation-range: 440vh 450vh; 
    }

    .ghost-text-2 {
        animation: ghost-fade-out linear forwards;
        animation-timeline: scroll(root);
        /* "BATS!" verdwijnt exact wanneer "HEY JOH!" (op 550vh) vastplakt */
        animation-range: 540vh 550vh; 
    }

    .ghost-text-3 {
        animation: ghost-fade-out linear forwards;
        animation-timeline: scroll(root);
        /* "HEY JOH!" verdwijnt exact wanneer "DIKKE W!" (op 650vh) vastplakt */
        animation-range: 640vh 650vh; 
    }

    /* DIKKE W! (Project 3) hoeft niet op deze manier, want die wordt met de card-shuffle hieronder het hele scherm uit geschoven! */


    /* Shuffle Out animaties (blijven lopen met de main content mee) */
    .ghost-work-title { z-index: 20; animation: ghost-card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 680vh 750vh; }
    .ghost-first-project { z-index: 30; animation: ghost-card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 670vh 750vh; }
    .ghost-second-project { z-index: 35; animation: ghost-card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 660vh 750vh; }
    .ghost-third-project { z-index: 40; animation: ghost-card-shuffle-out linear forwards; animation-timeline: scroll(root); animation-range: 650vh 750vh; }
    
    .ghost-about-section { z-index: 45; animation: ghost-zoom-out-and-round linear forwards; animation-timeline: scroll(root); animation-range: 750vh 900vh; }
    .ghost-contact-section { z-index: 50; }
    
    .ghost-about-clickable { display: flex; flex-direction: column; align-items: center; }
    .ghost-click-hint { margin-top: 1rem; opacity: 0.7; font-family: var(--extra-expanded-font); font-size: 1.2rem; }
    .ghost-contact-wrapper { text-align: center; }
    .ghost-contact-links { display: flex; gap: 2rem; justify-content: center; margin-top: 2rem; font-family: var(--extra-expanded-font); font-size: 1.5rem; }


    /* ========================================================= */
    /* 4. GHOST KEYFRAMES                                        */
    /* ========================================================= */
    
    /* Zorgt voor een snelle, onzichtbare fadeout van de teksten */
    @keyframes ghost-fade-out { 
        to { opacity: 0; visibility: hidden; } 
    }

    @keyframes ghost-move-to-center-and-scale {
        0% { transform: translate(0, 0) scale(1); top: 0%; left: 0%; }
        30% { transform: translate(-50%, -50%) scale(1); top: 50%; left: 50%; }
        50% { transform: translate(-50%, -50%) scale(5); top: 50%; left: 50%; }
        100% { transform: translate(-50%, -50%) scale(100); top: 50%; left: 50%; }
    }

    @keyframes ghost-card-shuffle-out { 
        0% { transform: translateX(0); } 
        100% { transform: translateX(120%) rotate(5deg); } 
    }

    @keyframes ghost-remove-section { 0% { opacity: 1; } 100% { opacity: 0; } }
    @keyframes ghost-zoom-out-and-round { 0% { transform: scale(1); border-radius: 0px; } 100% { transform: scale(0.8); border-radius: 60px; } }
</style>