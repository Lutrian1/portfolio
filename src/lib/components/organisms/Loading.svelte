<script>
    import { onMount, createEventDispatcher } from 'svelte';
    import { gsap } from 'gsap';

    const dispatch = createEventDispatcher();
    let progress = 0;
    let container;

    onMount(() => {
        // Creatieve, onregelmatige laad-animatie
        const loadingTimeline = gsap.to({}, {
            duration: 2.2,
            onUpdate: function() {
                // Maak de stappen een beetje 'glitchy' en non-lineair
                progress = Math.floor(this.progress() * 100);
            },
            ease: "power3.inOut",
            onComplete: () => {
                // Smooth fade-out van het hele laadscherm
                gsap.to(container, {
                    opacity: 0,
                    duration: 0.6,
                    ease: "power2.inOut",
                    onComplete: () => {
                        // Geef een seintje aan de hoofdpagina dat we klaar zijn
                        dispatch('complete');
                    }
                });
            }
        });
    });
</script>

<div bind:this={container} class="loader-container">
    <div class="loader-content">
        <span class="system-status">BOOTING INTERACTION NETWORK...</span>
        
        <div class="progress-bar-wrapper">
            <div class="progress-bar" style="width: {progress}%"></div>
        </div>
        
        <div class="counter-row">
            <span class="percentage">{progress}%</span>
            <span class="status-code">SYS_OK</span>
        </div>
    </div>
</div>

<style>
    .loader-container {
        position: fixed;
        inset: 0;
        background-color: #000;
        /* Absurde z-index zodat hij gegarandeerd over de WebGL canvas heen valt */
        z-index: 999999; 
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: monospace;
    }

    .loader-content {
        display: flex;
        flex-direction: column;
        width: clamp(280px, 40vw, 450px);
        gap: 1rem;
    }

    .system-status {
        color: #adff2f;
        font-size: 0.8rem;
        letter-spacing: 2px;
        opacity: 0.8;
        animation: pulse 1.5s infinite ease-in-out;
    }

    .progress-bar-wrapper {
        width: 100%;
        height: 6px;
        background: rgba(173, 255, 47, 0.05);
        border: 1px solid rgba(173, 255, 47, 0.2);
        transform: skewX(-10deg);
        overflow: hidden;
    }

    .progress-bar {
        height: 100%;
        background-color: #adff2f;
        box-shadow: 0 0 15px #adff2f;
        /* Zorgt dat de progressie vloeiend oogt */
        transition: width 0.05s linear; 
    }

    .counter-row {
        display: flex;
        justify-content: space-between;
        align-items: flex-end;
    }

    .percentage {
        color: #adff2f;
        font-size: 2.5rem;
        font-weight: 900;
        line-height: 1;
    }

    .status-code {
        color: #adff2f;
        font-size: 0.8rem;
        opacity: 0.5;
        letter-spacing: 1px;
    }

    @keyframes pulse {
        0%, 100% { opacity: 0.6; }
        50% { opacity: 1; }
    }
</style>