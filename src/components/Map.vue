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

      const path = "/img/dark-s_";
      const format = ".jpg";
      const urls = [
        path + "px" + format,
        path + "nx" + format,
        path + "py" + format,
        path + "ny" + format,
        path + "pz" + format,
        path + "nz" + format,
      ];
      this.scene.background = new THREE.CubeTextureLoader().load(urls);

      const geometry = new THREE.SphereGeometry(0.2, 22, 22);

      let mag = dataset.map((a) => a.mag);
      mag = mag.map(this.normalize(-1, 6));

      for (const index in dataset) {
        let mesh = new THREE.Mesh(geometry, new THREE.MeshLambertMaterial());
        mesh.position.set(dataset[index].x, dataset[index].y, dataset[index].z);
        mesh.scale.set(
          dataset[index].dist / 40,
          dataset[index].dist / 40,
          dataset[index].dist / 40
        );
        const ci = dataset[index].ci;
        if (ci < 0) {
          mesh.material.color = new THREE.Color("rgb(255,98,80)");
        } else if (ci < 0.2) {
          mesh.material.color = new THREE.Color("rgb(255,174,77)");
        } else if (ci < 0.5) {
          mesh.material.color = new THREE.Color("rgb(255,246,125)");
        } else if (ci < 1) {
          mesh.material.color = new THREE.Color("rgb(248,255,190)");
        } else if (ci < 1.5) {
          mesh.material.color = new THREE.Color("rgb(190,255,228)");
        } else if (ci < 2) {
          mesh.material.color = new THREE.Color("rgb(111,255,221)");
        } else {
          mesh.material.color = new THREE.Color("rgb(72,221,255)");
        }
        mesh.material.transparent = true;
        mesh.material.opacity = mag[index];
        mesh.userData = dataset[index];
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
        let message = `
          bf: "${this.intersected.userData.bf}"\n
          ci: ${this.intersected.userData.ci}\n
          dist: ${this.intersected.userData.dist}\n
          id: ${this.intersected.userData.id}\n
          lum: ${this.intersected.userData.lum}\n
          mag: ${this.intersected.userData.mag}\n
          proper: "${this.intersected.userData.proper}"\n
          x: ${this.intersected.userData.x}\n
          y: ${this.intersected.userData.y}\n
          z: ${this.intersected.userData.z}
        `;
        alert(message);
      }
    },
    normalize(min, max) {
      let delta = max - min;
      return function (val) {
        return (val - min) / delta;
      };
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
  height: 100vh;
  width: 100%;
}
</style>
