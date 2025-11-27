<script>
	import { gsap } from "gsap";
	import favicon from '$lib/assets/favicon.svg';
	import '$lib/css/global-styles.css';
	import { NoiseBackground } from '$lib';

	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
</svelte:head>

{@render children()}

<NoiseBackground />
