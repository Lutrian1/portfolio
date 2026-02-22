<script>
    import '$lib/components/atomns/wingItText.css';
</script>

<div aria-hidden="true" class="just-wing-it-box-variation-2">
    <div class="animation-variation-2">
        <div class="fake-text"></div>
        <div class="fake-text-2"></div>
    </div>
</div>

<style>
    .just-wing-it-box-variation-2 {
        /* Safari-specific fixes */
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden;
        -webkit-perspective: 1000;
        perspective: 1000;
        -webkit-transform: translateZ(0);
        transform: translateZ(0);
        
        /* Animation with both prefixes */
        -webkit-animation: moveDownAndFade linear forwards;
        animation: moveDownAndFade linear forwards;
        
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
    @-webkit-keyframes moveDownAndFade {
        0% {
            -webkit-transform: translateY(0);
            opacity: 1;
        }
        100% {
            -webkit-transform: translateY(100px);
            opacity: 0;
        }
    }

    @keyframes moveDownAndFade {
        0% {
            transform: translateY(0);
            opacity: 1;
        }
        100% {
            transform: translateY(100px);
            opacity: 0;
        }
    }

    /* Safari fallback using JavaScript detection */
    @supports (-webkit-overflow-scrolling: touch) {
        .just-wing-it-box-variation-2 {
            /* Alternative animation method for Safari */
            animation: none;
            -webkit-animation: moveDownAndFade linear forwards;
            -webkit-animation-timeline: scroll(root);
            -webkit-animation-range: 0vh 50vh;
        }
    }
</style>