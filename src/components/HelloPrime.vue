<template>
  
  <div id="container">


  </div>

</template>

<script>

import * as Three from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';


export default {
  name: 'HelloPrime',

  data(){
    return{
      scene: null,
      camera: null,
      renderer: null,
      mouse:new Three.Vector2(),
      rayCaster:new Three.Raycaster(),
    }
  },
  methods:{

    init: function() {

      //all the initialisation
      this.scene = new Three.Scene();
      let container = document.getElementById('container');

      //camera
      const width = 10;
      const height = width * (container.clientHeight/container.clientWidth);
      this.camera = new Three.OrthographicCamera(
        width /-2,
        width /2,
        height /2,
        height /-2,
        1,
        100
      );
      this.camera.position.set(10,10,10);
      this.camera.lookAt(0,0,0);

      //renderer
      this.renderer = new Three.WebGLRenderer({antialias: true});
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      this.renderer.setClearColor("white");
      container.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.05;
      this.controls.enableZoom = true;
      this.controls.update();

    },
    resizeRenderer: function() {
      let container = document.getElementById('container');
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      this.camera.aspect = container.clientWidth/container.clientHeight;
      this.camera.updateProjectionMatrix();
    },
    animate: function() {
      requestAnimationFrame(this.animate);
      this.renderer.render(this.scene, this.camera);
    },

  },
  mounted(){
      this.init();
      this.animate();
      window.addEventListener('resize', this.resizeRenderer)
  },

}
</script>
