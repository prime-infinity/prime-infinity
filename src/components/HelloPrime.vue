<template>
  
  <div>
    <div id="container">
    </div>

    <div class="" id="textFloat">

      <div class="appearOne">
        <h1 class="col-12" style="">prime-infinity</h1>
      </div>
      <div class="appearTwo">
        <h4 class="col-12" style="">The Universe in a man</h4>
      </div>
      
    </div>

  </div>
  
</template>

<script>

import * as Three from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import Stats from 'three/examples/js/libs/stats.min.js';
import { gsap } from "gsap";

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
      box:null,
      stars1:null,
      stars2:null,
      shootingS:null,
      radius:6371,
    }
  },
  methods:{
    beginTextAni:function(){

      setTimeout(() => {
        gsap.to('.appearOne', {duration: 3, opacity: 1})

        setTimeout(() => {
          gsap.to('.appearTwo', {duration: 3, opacity: 1})
        }, 2000);

      }, 3000);


      setTimeout(() => {

        gsap.to(this.shootingS.position, {duration: 20,y:-12,x:-12,ease:"Expo.easeOut"});
        
      }, 5000);
      
    },
    init: function() {

      //all the initialisation
      this.scene = new Three.Scene();
      let container = document.getElementById('container');
      
      //ambient light
      const aL = new Three.AmbientLight(0xffffff,1);
      this.scene.add(aL);

    
      //camera
      this.camera = new Three.PerspectiveCamera(75,container.clientWidth/container.clientHeight,0.1,1000);
      this.camera.position.x = 0
      this.camera.position.y = 0
      this.camera.position.z = this.radius * 5;

      //renderer
      this.renderer = new Three.WebGLRenderer({powerPreference: "high-performance",antialias: true, });
  
      this.renderer.setSize(container.clientWidth, container.clientHeight);
      this.renderer.setClearColor(0x000);
      container.appendChild(this.renderer.domElement);

      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.05;
      this.controls.enableZoom = true;
      this.controls.update();

      this.stats = new Stats()
      container.appendChild( this.stats.dom );

      const geometry = new Three.BoxGeometry( 1, 1, 1 );
      const material = new Three.MeshBasicMaterial( {color: 0x00ff00} );
      this.box = new Three.Mesh( geometry, material );
      //this.scene.add( this.box );


      //stars

      const r = this.radius;
      const starsGeometry = [ new Three.BufferGeometry(), new Three.BufferGeometry() ];  
      
      const vertices1 = [];
			const vertices2 = [];

      const vertex = new Three.Vector3();


      for ( let i = 0; i < 250; i ++ ) {

        vertex.x = Math.random() * 2 - 1;
        vertex.y = Math.random() * 2 - 1;
        vertex.z = Math.random() * 2 - 1;
        vertex.multiplyScalar( r );

        vertices1.push( vertex.x, vertex.y, vertex.z );

      }

      for ( let i = 0; i < 1500; i ++ ) {

        vertex.x = Math.random() * 2 - 1;
        vertex.y = Math.random() * 2 - 1;
        vertex.z = Math.random() * 2 - 1;
        vertex.multiplyScalar( r );

        vertices2.push( vertex.x, vertex.y, vertex.z );

      }

      starsGeometry[ 0 ].setAttribute( 'position', new Three.Float32BufferAttribute( vertices1, 3 ) );
      starsGeometry[ 1 ].setAttribute( 'position', new Three.Float32BufferAttribute( vertices2, 3 ) );
       
      const starsMaterials = [
        new Three.PointsMaterial( { color: 0x555555, size: 2, sizeAttenuation: false } ),
        new Three.PointsMaterial( { color: 0x555555, size: 1, sizeAttenuation: false } ),
        new Three.PointsMaterial( { color: 0x333333, size: 2, sizeAttenuation: false } ),
        new Three.PointsMaterial( { color: 0x3a3a3a, size: 1, sizeAttenuation: false } ),
        new Three.PointsMaterial( { color: 0x1a1a1a, size: 2, sizeAttenuation: false } ),
        new Three.PointsMaterial( { color: 0x1a1a1a, size: 1, sizeAttenuation: false } )
      ];

      for ( let i = 10; i < 30; i ++ ) {

        const stars = new Three.Points( starsGeometry[ i % 2 ], starsMaterials[ i % 6 ] );

        stars.rotation.x = Math.random() * 6;
        stars.rotation.y = Math.random() * 6;
        stars.rotation.z = Math.random() * 6;
        stars.scale.setScalar( i * 10 );

        stars.matrixAutoUpdate = false;
        stars.updateMatrix();
        
        this.scene.add( stars );

      }

      const geometrys = [new Three.BufferGeometry(), new Three.BufferGeometry()];
      geometrys[0].setAttribute(
        "position",
        new Three.BufferAttribute(this.getRandomParticelPos(300), 3)
      );
      geometrys[1].setAttribute(
        "position",
        new Three.BufferAttribute(this.getRandomParticelPos(300), 3)
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
      /*this.scene.add(this.stars1);
      this.scene.add(this.stars2);*/

      //shootingS
      const geometryy = new Three.ConeGeometry( 0.3, 15, 32 );
      const materialy = new Three.MeshBasicMaterial( {color: "white"} );
      this.shootingS = new Three.Mesh( geometryy, materialy );
      this.shootingS.position.set(6,6,-4)
      this.shootingS.scale.set(0.1,0.1,0.1);
      this.shootingS.rotation.set(0,0,2.2)
      this.scene.add( this.shootingS );

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

      this.stars1.position.x = this.mouse.x * 0.0002;
      this.stars1.position.y = this.mouse.y * -0.0002;
      this.stars2.position.x = this.mouse.x * 0.0001;
      this.stars2.position.y = this.mouse.y * -0.0001;

      this.stars1.rotation.z += 0.00015;
      this.stars2.rotation.z += 0.00025;

    },

  },
  mounted(){
      this.init();
      this.animate();
      window.addEventListener('resize', this.resizeRenderer)
      window.addEventListener('mousemove', this.mouseMoved)
      this.beginTextAni();
      
  },


}
</script>
