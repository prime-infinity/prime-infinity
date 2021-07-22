<template>
  
  <div id="container">


  </div>

</template>

<script>

import * as Three from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import Stats from 'three/examples/js/libs/stats.min.js';


export default {
  name: 'HelloPrime',

  data(){
    return{
      scene: null,
      camera: null,
      renderer: null,
      mouse:new Three.Vector2(),
      rayCaster:new Three.Raycaster(),
      stats:null,
      spheres:[],
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
      this.renderer = new Three.WebGLRenderer({powerPreference: "high-performance",antialias: true});
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      this.renderer.setClearColor(0xffffff, 0);
      container.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.05;
      this.controls.enableZoom = true;
      this.controls.update();

      this.stats = new Stats()
      container.appendChild( this.stats.dom );

      //galaxy
      var geoSphere = new Three.SphereGeometry(Math.random() * 1, 20, 20);
      for (let i = 0; i < 500; i++) {
        // randRadius = Math.random()*30+10;
        let lumiereS = new Three.MeshPhongMaterial({
            emissive: '#fff'
        });
        this.spheres.push(new Three.Mesh(geoSphere, lumiereS));
      }

      
      for (let i = 0; i < this.spheres.length; i++) {
        this.spheres[i].position.set(Math.random() * 600 - 300, Math.random() * 600 - 300, Math.random() * 600 - 300);
        this.scene.add(this.spheres[i]);
      }

    },
    resizeRenderer: function() {
      let container = document.getElementById('container');
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      this.camera.aspect = container.clientWidth/container.clientHeight;
      this.camera.updateProjectionMatrix();
    },
    animate: function() {
      requestAnimationFrame(this.animate);
      this.stats.begin();
      this.renderer.render(this.scene, this.camera);
			this.stats.end();
    },

  },
  mounted(){
      this.init();
      this.animate();
      window.addEventListener('resize', this.resizeRenderer)
  },

}
</script>
