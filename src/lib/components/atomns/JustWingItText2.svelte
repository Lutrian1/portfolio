<script>
    import { gsap } from "gsap";
    import '$lib/components/atomns/wingItText.css';

    let text2;
    
    $effect(() => {
        // Right scroll animation
        gsap.to(text2, {
            x: '50%',
            duration: 5,
            ease: 'none',
            repeat: -1 // infinite repeat
        });
    });
</script>

<p aria-hidden="true" class="just-wing-it-text variation2" bind:this={text2}>Just wing it</p>

