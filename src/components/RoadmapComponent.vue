<template>
    <div id="container"></div>
  </template>
  
  <script>
  import * as THREE from 'three';
  
  export default {
    name: 'RoadmapComponent',
    mounted() {
      this.initScene();
    },
    methods: {
      initScene() {
        // Create a scene, camera, and renderer
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ alpha: true });
  
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.getElementById('container').appendChild(renderer.domElement);
  
        // Create a simple road (a plane)
        const roadGeometry = new THREE.PlaneGeometry(20, 100);
        const roadMaterial = new THREE.MeshBasicMaterial({ color: 0x333333 });
        const road = new THREE.Mesh(roadGeometry, roadMaterial);
        road.rotation.x = -Math.PI / 2; // Rotate to lay flat
        scene.add(road);
  
        // Create a simple character (a box)
        const characterGeometry = new THREE.BoxGeometry(0.5, 1, 0.5);
        const characterMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
        const character = new THREE.Mesh(characterGeometry, characterMaterial);
        character.position.y = 0.5; // Lift the character above the ground
        character.position.z = -45; // Start position on the road
        scene.add(character);
  
        // Camera position
        camera.position.set(0, 5, 10);
        camera.lookAt(0, 0, 0);
  
        // Movement variables
        const moveSpeed = 0.1;
  
        // Keyboard input handling
        document.addEventListener('keydown', (event) => {
          if (event.key === 'ArrowUp') {
            character.position.z += moveSpeed; // Move forward
          }
          if (event.key === 'ArrowDown') {
            character.position.z -= moveSpeed; // Move backward
          }
        });
  
        // Animation loop
        const animate = () => {
          requestAnimationFrame(animate);
          renderer.render(scene, camera);
        };
        animate();
      }
    }
  };
  </script>
  
  <style>
  #container {
    border: 2px solid red; /* Temporary for visibility */
    width: 100vw;
    height: 100vh;
    background-color: #87CEEB; /* Sky blue */
    overflow: hidden; /* Prevent scrolling */
  }
  </style>
  
 