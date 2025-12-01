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
        <GradientMaskHeadText />
        <h3>Based in Haarlem</h3>
        <JustWingItText2 />
        <MyFigure3D />
    </article>
</section>

<style>
    :root{
        --border-radius-landing-card1: 3vw;
    }

    section{
        max-height: 100vh;
        overflow: clip;
        padding: calc(var(--grid-gap) * 4) var(--grid-gap);
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
        text-align: center;
        height: 13.536vw;
        font-size: var(--ultra-big-font-size);
        @media (min-width: 768px) {
            text-align: left;
        }
    }

    h3{
        margin-top: -1.6vw;
        font-size: clamp(1rem, 3.891vw, 7rem);
        position: absolute;
        text-align: center;
        width: 100%;
        bottom: 0;
        @media (min-width: 768px) {
            text-indent: calc(var(--grid-gap) * 2 + 1.06vw);
            display: grid;
            text-align: left;
            position: relative;
            margin-top: -0.6vw;
            font-size: clamp(1.5rem, 5.091vw, 7rem);
            &::after{
                text-indent: 9.7vw;
                grid-column: 10 / var(--grid-colomn-amount);
                content: 'Based in Haarlem';
            }
        }
    }

    article{
        position: relative;
        grid-column: 2 / var(--grid-colomn-amount);
        width: calc(100% + var(--grid-gap) * 2);
        left: calc(-1 * var(--grid-gap));
        border-radius: var(--border-radius-landing-card1);
        background-color: var(--color-neutral-100);
    }

</style>