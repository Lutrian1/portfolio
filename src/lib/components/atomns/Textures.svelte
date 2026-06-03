<script>
    import grungeTexture from '$lib/assets/images/grunge-texture.avif';
    import paintTexture from '$lib/assets/images/paint-texture.jpg';
</script>

<div class="noise-fg"></div>

<svg width="100%" height="100%" style="position:absolute;">
    <filter id='brush-distort' x='0%' y='0%' width='100%' height='100%'>
      <feTurbulence baseFrequency="0.01 0.2" result="NOISE" numOctaves="2"/>
	  <feDisplacementMap in="SourceGraphic" in2="NOISE" scale="20" xChannelSelector="R" yChannelSelector="R"></feDisplacementMap>  
    </filter>
</svg>

<img 
	src={grungeTexture} 
	alt=""
	class="grunge-texture"
/>

<img 
	src={paintTexture} 
	alt=""
	class="paint-texture"
/>

<style>
    
.noise-fg {
    position: fixed;
    pointer-events: none;
    top: -10rem;
    left: -10rem;
    animation: noise-animation .4s steps(2) infinite;
    background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' version='1.1' xmlns:xlink='http://www.w3.org/1999/xlink' xmlns:svgjs='http://svgjs.dev/svgjs' viewBox='0 0 700 700' width='700' height='700' opacity='1'%3E%3Cdefs%3E%3Cfilter id='nnnoise-filter' x='-20%25' y='-20%25' width='140%25' height='140%25' filterUnits='objectBoundingBox' primitiveUnits='userSpaceOnUse' color-interpolation-filters='linearRGB'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.056' numOctaves='4' seed='15' stitchTiles='stitch' x='0%25' y='0%25' width='100%25' height='100%25' result='turbulence'%3E%3C/feTurbulence%3E%3CfeSpecularLighting surfaceScale='25' specularConstant='8' specularExponent='30' lighting-color='%23FFFFFF' x='0%25' y='0%25' width='100%25' height='100%25' in='turbulence' result='specularLighting'%3E%3CfeDistantLight azimuth='3' elevation='88'%3E%3C/feDistantLight%3E%3C/feSpecularLighting%3E%3C/filter%3E%3C/defs%3E%3Crect width='700' height='700' fill='transparent'%3E%3C/rect%3E%3Crect width='700' height='700' fill='%23ffffff' filter='url(%23nnnoise-filter)'%3E%3C/rect%3E%3C/svg%3E") repeat 0 0;
    background-repeat: repeat;
    opacity: .4;
    background-position: 50%;
    width: calc(100% + 20rem);
    height: calc(100% + 20rem);
    content: "";
    z-index: 9999;
}

.grunge-texture, .paint-texture  {
    position: fixed;
    pointer-events: none;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    mix-blend-mode: hue;
    z-index: 9999;
}

.grunge-texture{
    opacity: .8;
}

.paint-texture{
    opacity: .15;
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