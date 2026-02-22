<div class="text-box-wrapper">
	<div class="text-box">
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
		<div>Web developer</div>
    </div>
</div>
<style>

@keyframes scroll-animation-mask-head-text {
	0% {
		transform: translateY(0) scale(1);
		opacity: 1;
	}
	100% {
		transform: translateY(-50%) scale(0.5);
		opacity: 0;
	}
}

.text-box-wrapper {
	inset:0;
	background-color: var(--color-neutral-100);
	height: 85%;
	mask-image: linear-gradient(var(--color-neutral-100) 1%,transparent 95%);
    overflow: hidden;
	position: absolute;
	top: 13.436vw;
	
	> div {
		height:100%;
	}
	
	div div{
		color: var(--h2-and-h3-color);
		font-family: var(--semi-condensed-font);
		text-transform: uppercase;
		text-align: center;
		font-weight: normal;
		height: 13.536vw;
		margin: 0;
		font-size: var(--ultra-big-font-size);
		
		animation-name: scroll-animation-mask-head-text;
		animation-fill-mode: forwards;
		animation-timeline: scroll(y root);
		animation-range: calc(var(--ultra-big-font-size) * sibling-index()) calc(var(--ultra-big-font-size) * (sibling-index() + 2));
	}

      @media (min-width: 768px) {
          display: none;
          > div{
              display: none;
          }
      }
}
</style>