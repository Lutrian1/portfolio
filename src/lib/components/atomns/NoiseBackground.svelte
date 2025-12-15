<div class="noise-bg"></div>

<svg width="0" height="0" style="position:absolute;">
    <filter id='brush-distort' x='0%' y='0%' width='100%' height='100%'>
      <feTurbulence baseFrequency="0.01 0.2" result="NOISE" numOctaves="2"/>
	  <feDisplacementMap in="SourceGraphic" in2="NOISE" scale="20" xChannelSelector="R" yChannelSelector="R"></feDisplacementMap>  
    </filter>
</svg>

<style>
    
.noise-bg {
    position: fixed;
    opacity: 1.5;
    top: -50%;
    left: -50%;
    right: -50%;
    bottom: -50%;
    width: 200%;
    height: 200vh;
    background: transparent url('http://assets.iceable.com/img/noise-transparent.png') repeat 0 0;
    background-repeat: repeat;
    animation: noise-animation .2s infinite;
    z-index: 5000;
    pointer-events: none;
}

@keyframes noise-animation {
    0% { transform: translate(0,0) }
    10% { transform: translate(-2%,-2%) }
    20% { transform: translate(-5%,7%) }
    30% { transform: translate(7%,-8%) }
    40% { transform: translate(-7%,10%) }
    50% { transform: translate(-10%,5%) }
    60% { transform: translate(10%,0) }
    70% { transform: translate(0,10%) }
    80% { transform: translate(-10%,0) }
    90% { transform: translate(10%,5%) }
    100% { transform: translate(5%,0) }
}

</style>