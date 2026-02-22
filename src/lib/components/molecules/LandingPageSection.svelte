<script>
    import { GradientMaskHeadText } from '$lib';
    import { JustWingItText1 } from '$lib';
    import { JustWingItText2 } from '$lib';
    import { MyFigure3D } from '$lib';
</script>

<section>
    <article>
        <JustWingItText1 />
        <h2>Web developer</h2>
        <span class="background-h2"></span>
        <GradientMaskHeadText />
        <h3 class="based-haarlem-text">Based in Haarlem<span class="copy-text-fake" aria-hidden="true">Based in Haarlem</span></h3>
        <JustWingItText2 />
        <MyFigure3D />
    </article>
</section>

<style>
    :root{
        --border-radius-landing-card1: 3vw;
    }

    section{
        min-height: calc(100% - calc(var(--grid-gap) * 8));
        max-height: 100%;
        overflow: clip;
        padding: calc(var(--grid-gap) * 4) 0;
        position: fixed;
        display: grid;
        grid-template-columns: var(--grid-columns);
        width: 100%;
        left: 0;
        @media (min-width: 768px) {
            min-height: 90%;
        }
    }

    article{
        z-index: -10;
        position: relative;
        grid-column: 2 / var(--grid-colomn-amount);
        width: calc(100% + var(--grid-gap) * 2);
        left: calc(-1 * var(--grid-gap));
        border-radius: var(--border-radius-landing-card1);
        background-color: var(--color-neutral-100);
    }

    h2, h3{
        color: var(--h2-and-h3-color);
        font-family: var(--semi-condensed-font);
        text-transform: uppercase;
        font-weight: normal;
        margin: 0;
        @media (min-width: 768px) {
            text-indent: calc(var(--grid-gap) * 2);
        }
    }

    h2{
        z-index: 100;
        text-align: center;
        position: relative;
        height: 13.536vw;
        font-size: var(--ultra-big-font-size);
        @media (min-width: 768px) {
            text-align: left;
        }
    }
    .background-h2{
        z-index: -1;
        position: absolute;
        left: 0;
        top: 0;
        background-color: var(--color-neutral-100);
        width: 100%;
        height: 13.436vw;
        border-radius: var(--border-radius-landing-card1) var(--border-radius-landing-card1) 0 0;
    }

    /* Based in Haarlem text animation */
    .based-haarlem-text {
        z-index: -8;
        margin-top: -1.6vw;
        font-size: clamp(1rem, 3.891vw, 7rem);
        position: absolute;
        text-align: center;
        width: 100%;
        bottom: 0;
        animation: none;
        
        @media (min-width: 768px) {
            animation: animation-move-up-and-fade linear forwards;
            animation-timeline: scroll(root);
            animation-range: 30vh 75vh; /* Animate over first 200vh of scroll */
            text-indent: calc(var(--grid-gap) * 2 + 1.06vw);
            display: grid;
            text-align: left;
            position: relative;
            margin-top: -0.6vw;
            font-size: clamp(1.5rem, 5.091vw, 7rem);
        }
        
    }

    /* Keyframes for the move up and fade animation */
    @keyframes animation-move-up-and-fade {
        0% {
            transform: translateY(0);
            opacity: 1;
        }
        50% {
            transform: translateY(-60px);
            opacity: 0.5;
        }
        100% {
            transform: translateY(-120px);
            opacity: 0;
        }
    }

    .copy-text-fake{
        display: none;
        @media (min-width: 768px) {
            display: block;
            text-indent: 9.7vw;
            grid-column: 10 / var(--grid-colomn-amount);
        }
    }

    /* When JS is enabled, disable the CSS animation */
    :global(.js-enabled) .based-haarlem-text {
        animation: none;
    }
</style>