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
      stars1:null,
      stars2:null,
    }
  },
  methods:{

    init: function() {

      //all the initialisation
      this.scene = new Three.Scene();
      let container = document.getElementById('container');

      // light source
      const color = 0xffffff, intensity = 1;
      const light = new Three.DirectionalLight(color, intensity);
      light.position.set(-1, 2, 4);
      this.scene.add(light);

    
      //camera
      this.camera = new Three.PerspectiveCamera(75,container.clientWidth/container.clientHeight,0.1,1000);
      this.camera.position.x = 0
      this.camera.position.y = 0
      this.camera.position.z = 2
      this.camera.lookAt(0, 0, 0)

      //renderer
      this.renderer = new Three.WebGLRenderer({powerPreference: "high-performance",antialias: true,});
  
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      this.renderer.setClearColor(new Three.Color("#1c1624"));
      container.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.05;
      this.controls.enableZoom = true;
      this.controls.update();

      this.stats = new Stats()
      container.appendChild( this.stats.dom );

      const geometrys = [new Three.BufferGeometry(), new Three.BufferGeometry()];


      geometrys[0].setAttribute(
        "position",
        new Three.BufferAttribute(this.getRandomParticelPos(350), 3)
      );
      geometrys[1].setAttribute(
        "position",
        new Three.BufferAttribute(this.getRandomParticelPos(1500), 3)
      );


      const loader = new Three.TextureLoader();

      const materials = [
        new Three.PointsMaterial({
          size: 0.05,
          map: loader.load(
            "/textures/sp1.png"
          ),
          transparent: true
        }),
        new Three.PointsMaterial({
          size: 0.075,
          map: loader.load(
            "/textures/sp2.png"
          ),
          transparent: true
        })
      ];

      this.stars1 = new Three.Points(geometrys[0], materials[0]);
      this.stars2 = new Three.Points(geometrys[1], materials[1]);
      this.scene.add(this.stars1);
      this.scene.add(this.stars2);

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

      /*this.box.rotation.x = this.mouse.x * 0.005;
      this.box.rotation.y = this.mouse.y * 0.005;*/

      this.stars1.position.x = this.mouse.x * 0.0001;
      this.stars1.position.y = this.mouse.y * -0.0001;

      this.stars2.position.x = this.mouse.x * 0.0001;
      this.stars2.position.y = this.mouse.y * -0.0001;

    },
    mouseMoved:function(e){
      this.mouse.x = e.clientX 
      this.mouse.y = e.clientY 
    },
    getRandomParticelPos:function(particleCount){
      const arr = new Float32Array(particleCount * 3);
        for (let i = 0; i < particleCount; i++) {
          arr[i] = (Math.random() - 0.5) * 10;
        }
        return arr;
    }
  },
  mounted(){
      this.init();
      this.animate();
      window.addEventListener('resize', this.resizeRenderer)
      window.addEventListener('mousemove', this.mouseMoved)
  },


}
</script>
