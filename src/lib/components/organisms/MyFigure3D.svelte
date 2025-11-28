<script>
	import { browser } from '$app/environment'; 
	import * as THREE from "three"

	if(browser) {
		let camera;
		let scene;
		let renderer;
		let cube;

		const init = () => {
			const canvas = document.getElementById('threeCanvas');
			
			scene = new THREE.Scene();
			
			// Get the actual display size of the canvas
			const width = canvas.clientWidth;
			const height = canvas.clientHeight;
			
			camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);

			renderer = new THREE.WebGLRenderer({ 
				canvas: canvas,
				antialias: true,
				alpha: true  // This makes the canvas background transparent
			});
			
			// Set the renderer size
			renderer.setSize(width, height, false);
			
			// Optional: Set clear color with transparency (0 as alpha)
			renderer.setClearColor(0x000000, 0); // Black with 0 alpha = transparent

			const geometry = new THREE.BoxGeometry(1, 1, 1);
			const material = new THREE.MeshBasicMaterial({ 
				color: 0xffffff,  // Your object stays white and visible
				transparent: false // Keep this false so object isn't transparent
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

<style>
	#threeCanvas {
        position: absolute;
        top: 25%;
        left: 25%;
		width: 50%;
        height: 50%;
		display: block;
		/* The canvas itself will be transparent */
	}
</style>