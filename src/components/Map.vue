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
      intersected: undefined,
      star: {
        bf: "",
        ci: 0,
        dist: 0,
        id: 0,
        lum: 0,
        mag: "",
        proper: "",
        x: 0,
        y: 0,
        z: 0,
      },
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
      this.pointer = new THREE.Vector2();
      this.raycaster = new THREE.Raycaster();

      const light = new THREE.AmbientLight(0xffffff, 2);
      this.scene.add(light);

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

      this.raycaster.setFromCamera(this.pointer, this.camera);
      const intersects = this.raycaster.intersectObjects(
        this.scene.children,
        false
      );
      if (intersects.length > 0) {
        if (this.intersected !== intersects[0].object) {
          if (this.intersected) {
            this.intersected.material.emissive.setHex(
              this.intersected.currentHex
            );
          }
          this.intersected = intersects[0].object;
          this.intersected.currentHex =
            this.intersected.material.emissive.getHex();
          this.intersected.material.emissive.setHex(0xffffff);
        }
      } else {
        if (this.intersected) {
          this.intersected.material.emissive.setHex(
            this.intersected.currentHex
          );
        }
        this.intersected = undefined;
      }

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
    onDocumentMouseMove(event) {
      this.pointer.x = (event.clientX / window.innerWidth) * 2 - 1;
      this.pointer.y = -(event.clientY / window.innerHeight) * 2 + 1;
    },
    onDocumentClick() {
      if (this.intersected) {
        alert(JSON.stringify(this.intersected.userData));
      }
    },
  },
  mounted() {
    this.init();
    this.animate();
    window.addEventListener("resize", this.onWindowResize);
    document.addEventListener("mousemove", this.onDocumentMouseMove);
    document.addEventListener("mousewheel", this.onDocumentMouseWheel);
    document.addEventListener("click", this.onDocumentClick);
  },
  beforeUnmount() {
    document.removeEventListener("click", this.onDocumentClick);
    document.removeEventListener("mousewheel", this.onDocumentMouseWheel);
    document.removeEventListener("mousemove", this.onDocumentMouseMove);
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
