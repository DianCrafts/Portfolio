<template>
    <div ref="threeContainer" class="three-container"></div>
  </template>
  
  <script>
  import * as THREE from 'three';
  import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
  
  export default {
    name: 'ThreeDScene',
    data() {
      return {
        scene: null,
        camera: null,
        renderer: null,
        humanModel: null,
      };
    },
    mounted() {
      this.initThreeJsScene();
      this.loadHumanModel();
      this.animate();
      window.addEventListener('keydown', this.onKeyDown); // Listen for keyboard events
    },
    beforeDestroy() {
      window.removeEventListener('keydown', this.onKeyDown);
    },
    methods: {
      initThreeJsScene() {
        // Create Scene
        this.scene = new THREE.Scene();
  
        // Create a Camera
        this.camera = new THREE.PerspectiveCamera(
          75,
          window.innerWidth / window.innerHeight,
          0.1,
          1000
        );
        this.camera.position.set(0, 5, 10); // Adjust the camera position for your scene
  
        // Create Renderer and add to DOM
        this.renderer = new THREE.WebGLRenderer({ antialias: true });
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.$refs.threeContainer.appendChild(this.renderer.domElement);
  
        // Add Lighting
        const light = new THREE.DirectionalLight(0xffffff, 1);
        light.position.set(0, 10, 10);
        this.scene.add(light);
  
        // Add a ground plane (this will represent your roadmap)
        const textureLoader = new THREE.TextureLoader();
        const roadTexture = textureLoader.load(require('@/assets/roadmap.png'));
        const planeGeometry = new THREE.PlaneGeometry(20, 20);
        const planeMaterial = new THREE.MeshBasicMaterial({ map: roadTexture });
        const plane = new THREE.Mesh(planeGeometry, planeMaterial);
        plane.rotation.x = -Math.PI / 2;
        this.scene.add(plane);
      },
  
      loadHumanModel() {
        // Load a 3D human model using GLTFLoader
        const loader = new GLTFLoader();
        loader.load(
          require('@/assets/human.glb'), // Make sure you have a human model in assets folder
          (gltf) => {
            this.humanModel = gltf.scene;
            this.humanModel.scale.set(0.5, 0.5, 0.5); // Scale down the human model
            this.humanModel.position.set(0, 0, 0); // Set initial position
            this.scene.add(this.humanModel);
          },
          undefined,
          (error) => {
            console.error('Error loading human model:', error);
          }
        );
      },
  
      animate() {
        requestAnimationFrame(this.animate);
        this.renderer.render(this.scene, this.camera);
      },
  
      onKeyDown(event) {
        if (!this.humanModel) return;
        const speed = 0.1;
        switch (event.key) {
          case 'ArrowUp':
            this.humanModel.position.z -= speed; // Move forward
            break;
          case 'ArrowDown':
            this.humanModel.position.z += speed; // Move backward
            break;
          case 'ArrowLeft':
            this.humanModel.position.x -= speed; // Move left
            break;
          case 'ArrowRight':
            this.humanModel.position.x += speed; // Move right
            break;
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .three-container {
    width: 100vw;
    height: 100vh;
  }
  </style>
  