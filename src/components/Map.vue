<template>
  <div id="threejs"></div>
</template>

<script>
import * as THREE from "three";
import dataset from "@/assets/json/stars.json";
import { OrbitControls } from "three/addons/controls/OrbitControls";

export default {
  name: "MapComponent",
  data() {
    return {
      container: undefined,
    };
  },
  methods: {
    init() {
      this.container = document.getElementById("threejs");

      this.camera = new THREE.PerspectiveCamera(
        80,
        this.container.clientWidth / this.container.clientHeight,
        0.01,
        10000
      );
      this.camera.position.set(0, 0, 0);

      this.scene = new THREE.Scene();

      const geometry = new THREE.SphereGeometry(0.2, 26, 26);
      //const material = new THREE.MeshNormalMaterial();

      for (const star of dataset) {
        let mesh = new THREE.Mesh(
          geometry,
          new THREE.MeshLambertMaterial({ color: Math.random() * 0xffffff })
        );
        mesh.position.set(star.x, star.y, star.z);
        mesh.userData = star;
        this.scene.add(mesh);
      }

      this.renderer = new THREE.WebGLRenderer({ antialias: true });
      this.renderer.setSize(
        this.container.clientWidth,
        this.container.clientHeight
      );
      this.container.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement);
      this.controls.target.set(0, 0.000000000000001, 0);
      this.controls.rotateSpeed = 0.5;
      this.controls.enableDamping = true;
      this.controls.enableZoom = false;
      this.controls.update();
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
      this.controls.update();
    },
    onWindowResize() {
      this.camera.aspect =
        this.container.clientWidth / this.container.clientHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(
        this.container.clientWidth,
        this.container.clientHeight
      );
    },
    onDocumentMouseWheel(event) {
      let fovMAX = 120;
      let fovMIN = 1;

      this.camera.fov -= event.wheelDeltaY * 0.05;
      this.camera.fov = Math.max(Math.min(this.camera.fov, fovMAX), fovMIN);
      this.camera.updateProjectionMatrix();
    },
  },
  mounted() {
    this.init();
    this.animate();
    window.addEventListener("resize", this.onWindowResize);
    document.addEventListener("mousewheel", this.onDocumentMouseWheel);
  },
  beforeUnmount() {
    document.removeEventListener("mousewheel", this.onDocumentMouseWheel);
    window.removeEventListener("resize", this.onWindowResize);
  },
};
</script>

<style lang="scss" scoped>
#threejs {
  width: 100%;
  height: 100vh;
}
</style>
