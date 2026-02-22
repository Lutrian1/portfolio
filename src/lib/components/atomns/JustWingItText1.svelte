<script>
    import '$lib/components/atomns/wingItText.css';
</script>

<div aria-hidden="true" class="just-wing-it-box-variation-1">
    <div class="animation-variation-1">
        <div class="fake-text"></div>
        <div class="fake-text-2"></div>
    </div>
</div>

<style>
    /* Animation for just-wing-it-box-variation-1 */
    .just-wing-it-box-variation-1 {
        animation: moveUpAndFade linear forwards;
        animation-timeline: scroll(root);
        animation-range: 0vh 30vh; /* Animate over first 100vh of scroll */
    }

    @keyframes moveUpAndFade {
        0% {
            transform: translateY(0);
            opacity: 1;
        }
        100% {
            transform: translateY(-100px);
            opacity: 0;
        }
    }

    /* When JS is enabled, disable the CSS animation */
    :global(.js-enabled) .just-wing-it-box-variation-1 {
        animation: none;
    }
</style>