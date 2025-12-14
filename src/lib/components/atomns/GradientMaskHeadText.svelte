<script>
  let numberOfDivs = $state(10); 
  
  $effect(() => {
    const updateDivCount = () => {
      const viewportWidth = window.innerWidth;
      
      // Adjust these breakpoints based on your testing
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
    
    return () => {
      window.removeEventListener('resize', updateDivCount);
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
        margin-top: -2vw;
        height: 13.536vw;
        text-align: center;
        color: var(--h2-and-h3-color);
        font-family: var(--semi-condensed-font);
        font-size: var(--ultra-big-font-size);
        text-transform: uppercase;
        
        &::before{
            content: 'Web Developer';      
        }
        
        @media (min-width: 768px) {
            display: none;
        }
    }
</style>