<script>
	import { browser } from '$app/environment'; 
	import * as THREE from "three";
	import fallBackImage from '$lib/assets/images/logo-fallback-image-nojs.png';

	if(browser) {
		// Hide fallback image
		const fallbackImg = document.querySelector('.fallback-image');
		fallbackImg.style.display = 'none';
		
		let camera;
		let scene;
		let renderer;
		let cube;
		
		// Flag to track initialization
		let isInitialized = false;

		const init = () => {
			const canvas = document.getElementById('threeCanvas');
			const loadingScreen = document.getElementById('loading-screen');
			
			scene = new THREE.Scene();
			const width = canvas.clientWidth;
			const height = canvas.clientHeight;
			
			camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
			renderer = new THREE.WebGLRenderer({ 
				canvas: canvas,
				antialias: true,
				alpha: true
			});
			
			renderer.setSize(width, height, false);
			renderer.setClearColor(0x000000, 0);

			const geometry = new THREE.BoxGeometry(1, 1, 1);
			const material = new THREE.MeshBasicMaterial({ 
				color: 0xffffff
			});
			cube = new THREE.Mesh(geometry, material);
			scene.add(cube);

			camera.position.z = 5;
			
			isInitialized = true;
			
			// Hide loading screen after a short delay (so user can see it)
			// Remove this if you don't want to show loading screen at all
			setTimeout(() => {
				loadingScreen.style.opacity = '0';
				setTimeout(() => {
					loadingScreen.style.display = 'none';
				}, 500);
			}, 500);
		}

		const render = () => {
			if (isInitialized) {
				renderer.render(scene, camera);
			}
		}

		const animate = () => {
			requestAnimationFrame(animate);

			if (cube) {
				cube.rotation.x += 0.005;
				cube.rotation.y += 0.005;
			}

			render();
		}

		const handleResize = () => {
			const canvas = document.getElementById('threeCanvas');
			const width = canvas.clientWidth;
			const height = canvas.clientHeight;
			
			if (camera) {
				camera.aspect = width / height;
				camera.updateProjectionMatrix();
				renderer.setSize(width, height, false);
			}
		}

		window.addEventListener('resize', handleResize);

		init();
		animate();
	}
</script>

<!-- Simple loading screen - only shows briefly -->
<div id="loading-screen" class="loading-screen">
	<div class="loading-spinner"></div>
</div>

<canvas id="threeCanvas"></canvas>

<img 
	src={fallBackImage} 
	alt=""
	class="fallback-image"
/>

<style>
	#threeCanvas, .fallback-image {
		position: absolute;
	}

	#threeCanvas{
		top: 25%;
		left: 25%;
		width: 50%;
		height: 50%;
		display: block;
	}
	
	.fallback-image {
		object-fit: contain;
		width: 95%;
		height: 95%;
		top: 5%;
		left: 1%;
		z-index: 500;
	}
	
	/* Minimal loading screen */
	.loading-screen {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0);
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 9999;
		opacity: 1;
		transition: opacity 0.5s ease;
	}
	
	.loading-spinner {
		border: 4px solid rgba(255, 255, 255, 0.3);
		border-radius: 50%;
		border-top: 4px solid white;
		width: 40px;
		height: 40px;
		animation: spin 1s linear infinite;
	}
	
	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}
</style>

<noscript>
	<style>
		#loading-screen {
			display: none !important;
		}
		.fallback-image {
			display: block !important;
		}
	</style>
</noscript>