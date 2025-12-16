<script>
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  
  let numberOfDivs = $state(10);
  let divElements = []; // Store references to all rendered divs
  
  $effect(() => {
    const updateDivCount = () => {
      const viewportWidth = window.innerWidth;
      
      if (viewportWidth < 300) numberOfDivs = 22;
      else if (viewportWidth < 350) numberOfDivs = 19;
      else if (viewportWidth < 400) numberOfDivs = 16;
      else if (viewportWidth < 450) numberOfDivs = 14;
      else if (viewportWidth < 500) numberOfDivs = 13;
      else if (viewportWidth < 600) numberOfDivs = 11;
      else if (viewportWidth < 700) numberOfDivs = 9;
      else if (viewportWidth < 800) numberOfDivs = 8;
      else numberOfDivs = 4;
    };
    
    updateDivCount();
    window.addEventListener('resize', updateDivCount);
    
    // GSAP Animation
    gsap.registerPlugin(ScrollTrigger);
    
    // Function to animate all current divs
    const animateDivs = () => {
      // Get ALL currently rendered divs
      const currentDivs = Array.from(document.querySelectorAll('[class^="fake-gradient-mask-text-"]'));
      
      if (currentDivs.length > 0) {
        // Kill any existing animations first
        ScrollTrigger.getAll().forEach(trigger => {
          if (trigger.animation && trigger.animation.vars.targets === currentDivs) {
            trigger.kill();
          }
        });
        
        // Create new animation for ALL divs
        gsap.to(currentDivs, {
          scrollTrigger: {
            trigger: "main",
            start: "top top",
            end: "top+=300 top",
            scrub: 0.7,
            // markers: true
          },
          y: -50,
          opacity: 0,
          scale: 0.8,
          duration: 1.2,
          ease: "power2.inOut",
          stagger: 0.02 // Even smaller stagger for many elements
        });
        
      }
    };
    
    // Initialize animation
    animateDivs();
    
    // Re-animate when numberOfDivs changes (on resize)
    $effect(() => {
      // This runs whenever numberOfDivs changes
      setTimeout(() => {
        animateDivs();
      }, 50);
    });
    
    return () => {
      window.removeEventListener('resize', updateDivCount);
      ScrollTrigger.getAll().forEach(trigger => trigger.kill());
    };
  });
  
  function calculateOpacity(index, totalDivs) {
    const minOpacity = 5;
    const maxOpacity = 90;
    
    if (totalDivs === 1) return maxOpacity; 
    
    const opacityRange = maxOpacity - minOpacity;
    const opacityStep = opacityRange / (totalDivs - 1);
    
    return maxOpacity - (opacityStep * index);
  }
</script>

{#each Array(22) as _, i}
    {#if i < numberOfDivs}
        <div 
          class="fake-gradient-mask-text-var-{(i % 9) + 1}"
          style="opacity: {calculateOpacity(i, numberOfDivs)}%"
        ></div>
    {/if}
{/each}

<style>
    div {
        z-index: -8;
        margin-top: -2vw;
        height: 13.536vw;
        text-align: center;
        color: var(--h2-and-h3-color);
        font-family: var(--semi-condensed-font);
        font-size: var(--ultra-big-font-size);
        text-transform: uppercase;
        will-change: transform, opacity;
        
        &::before{
            content: 'Web Developer';      
        }
        
        @media (min-width: 768px) {
            display: none;
        }
    }
</style>