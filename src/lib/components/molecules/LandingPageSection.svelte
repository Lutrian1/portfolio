<script>
    import { JustWingItText1 } from '$lib';
    import { JustWingItText2 } from '$lib';
</script>

<section>
    <article>
        <JustWingItText1 />
        <h2>Web developer</h2>
        <h3>Based in Haarlem</h3>
        <JustWingItText2 />
    </article>
</section>

<style>
    :root{
        --border-radius-landing-card1: 3vw;
    }

    section{
        overflow: clip;
        padding: calc(var(--grid-gap) * 4) var(--grid-gap);
    }

    h2, h3{
        text-indent: calc(var(--grid-gap) * 2);
        color: var(--h2-and-h3-color);
        font-family: var(--semi-condensed-font);
        text-transform: uppercase;
        font-weight: normal;
        margin: 0;
    }

    h2{
        height: 13.536vw;
        font-size: clamp(2.125rem, 16.436vw, 30.5rem);
    }

    h3{
        margin-top: -0.6vw;
        text-indent: calc(var(--grid-gap) * 2 + 1.06vw);
        font-size: clamp(1.5rem, 5.091vw, 7rem);
        display: grid;
        position: relative;
        &::after{
            text-indent: 9.7vw;
            grid-column: 10 / var(--grid-colomn-amount);
            content: 'Based in Haarlem';
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