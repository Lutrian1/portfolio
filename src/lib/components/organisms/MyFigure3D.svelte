<script>
	import { browser } from '$app/environment'; 
	import * as THREE from "three";
	import fallBackImage from '$lib/assets/images/logo-fallback-image-nojs.png';

	if(browser) {
		// JavaScript is enabled - hide the fallback image
		const fallbackImg = document.querySelector('.fallback-image');
		fallbackImg.style.display = 'none';
		
		let camera;
		let scene;
		let renderer;
		let cube;

		const init = () => {
			const canvas = document.getElementById('threeCanvas');
			
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
		}

		const render = () => {
			renderer.render(scene, camera);
		}

		const animate = () => {
			requestAnimationFrame(animate);

			cube.rotation.x += 0.005;
			cube.rotation.y += 0.005;

			render();
		}

		const handleResize = () => {
			const canvas = document.getElementById('threeCanvas');
			const width = canvas.clientWidth;
			const height = canvas.clientHeight;
			
			camera.aspect = width / height;
			camera.updateProjectionMatrix();
			renderer.setSize(width, height, false);
		}

		window.addEventListener('resize', handleResize);

		init();
		animate();
	}
</script>

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
</style>