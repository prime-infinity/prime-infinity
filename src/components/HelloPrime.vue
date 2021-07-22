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
      t1:null,
      mouse:new Three.Vector2(),
      rayCaster:new Three.Raycaster(),
    }
  },
  methods:{

    init: function() {

      //all the initialisation
      this.scene = new Three.Scene();
      let container = document.getElementById('container');

      //ambient light
      const aL = new Three.AmbientLight(0xffffff,0.6);
      this.scene.add(aL);

      //directional light
      const dL = new Three.DirectionalLight(0xffffff,0.5);
      dL.position.set(10,20,0);
      this.scene.add(dL);

      //camera
      this.camera = new Three.PerspectiveCamera(75,container.clientWidth/container.clientHeight,0.1,1000);
      this.camera.position.set(0,0,10);

      //renderer
      this.renderer = new Three.WebGLRenderer({antialias: true});
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      this.renderer.setClearColor(0x151616);
      container.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.05;
      this.controls.enableZoom = true;
      this.controls.update();

      //first pyramid
      const t1Geo = new Three.ConeGeometry( 2, 3, 4 );
      const t1Mat = new Three.MeshLambertMaterial( {color: 0x5E6262} );
      this.t1 = new Three.Mesh( t1Geo, t1Mat );
      this.t1.position.set(0,0,0);
      this.t1.scale.set(1,1,1);
      this.scene.add(this.t1);
     

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
