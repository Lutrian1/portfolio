<div class="noise-fg"></div>

<svg width="0" height="0" style="position:absolute;">
    <filter id='brush-distort' x='0%' y='0%' width='100%' height='100%' color-interpolation-filters="sRGB">
        <feTurbulence baseFrequency="0.015 0.25" result="NOISE" numOctaves="3" seed="2"/>
        
        <feDisplacementMap 
            in="SourceGraphic" 
            in2="NOISE" 
            scale="15" 
            xChannelSelector="R" 
            yChannelSelector="G"
        />
        
        <feDisplacementMap 
            in="SourceGraphic" 
            in2="NOISE" 
            scale="10" 
            xChannelSelector="G" 
            yChannelSelector="B"
        />
    </filter>
</svg>

<style>
    
.noise-fg {
    position: fixed;
    pointer-events: none;
    top: -10rem;
    left: -10rem;
    animation: noise-animation .4s steps(2) infinite;
    background: url('http://assets.iceable.com/img/noise-transparent.png') repeat 0 0;
    background-repeat: repeat;
    background-position: 50%;
    width: calc(100% + 20rem);
    height: calc(100% + 20rem);
    content: "";
    z-index: 50000;
}

@keyframes noise-animation {
    0% {
        -webkit-transform: translate3d(0,9rem,0);
        transform: translate3d(0,9rem,0)
    }

    10% {
        -webkit-transform: translate3d(-1rem,-4rem,0);
        transform: translate3d(-1rem,-4rem,0)
    }

    20% {
        -webkit-transform: translate3d(-8rem,2rem,0);
        transform: translate3d(-8rem,2rem,0)
    }

    30% {
        -webkit-transform: translate3d(9rem,-9rem,0);
        transform: translate3d(9rem,-9rem,0)
    }

    40% {
        -webkit-transform: translate3d(-2rem,7rem,0);
        transform: translate3d(-2rem,7rem,0)
    }

    50% {
        -webkit-transform: translate3d(-9rem,-4rem,0);
        transform: translate3d(-9rem,-4rem,0)
    }

    60% {
        -webkit-transform: translate3d(2rem,6rem,0);
        transform: translate3d(2rem,6rem,0)
    }

    70% {
        -webkit-transform: translate3d(7rem,-8rem,0);
        transform: translate3d(7rem,-8rem,0)
    }

    80% {
        -webkit-transform: translate3d(-9rem,1rem,0);
        transform: translate3d(-9rem,1rem,0)
    }

    90% {
        -webkit-transform: translate3d(6rem,-5rem,0);
        transform: translate3d(6rem,-5rem,0)
    }

    to {
        -webkit-transform: translate3d(-7rem,0,0);
        transform: translate3d(-7rem,0,0)
    }
}
</style>