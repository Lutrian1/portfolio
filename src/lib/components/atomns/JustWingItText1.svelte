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
    .just-wing-it-box-variation-1 {
        /* Safari-specific fixes */
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden;
        -webkit-perspective: 1000;
        perspective: 1000;
        -webkit-transform: translateZ(0);
        transform: translateZ(0);
        
        /* Animation with both prefixes */
        -webkit-animation: moveUpAndFade linear forwards;
        animation: moveUpAndFade linear forwards;
        
        /* Bind to scroll */
        -webkit-animation-timeline: scroll(root);
        animation-timeline: scroll(root);
        
        /* Animation range */
        -webkit-animation-range: 0vh 50vh;
        animation-range: 0vh 50vh;
        
        /* Optimize performance */
        will-change: transform, opacity;
    }

    /* Keyframes with both prefixes */
    @-webkit-keyframes moveUpAndFade {
        0% {
            -webkit-transform: translateY(0);
            opacity: 1;
        }
        100% {
            -webkit-transform: translateY(-100px);
            opacity: 0;
        }
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

    /* Safari fallback using JavaScript detection */
    @supports (-webkit-overflow-scrolling: touch) {
        .just-wing-it-box-variation-1 {
            /* Alternative animation method for Safari */
            animation: none;
            -webkit-animation: moveUpAndFade linear forwards;
            -webkit-animation-timeline: scroll(root);
            -webkit-animation-range: 0vh 50vh;
        }
        
    }
</style>