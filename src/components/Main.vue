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

import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import Stats from 'three/examples/js/libs/stats.min.js';
import { gsap } from "gsap";

export default {
    name: 'Main',

    data(){
        return{
            scene: null,
            camera: null,
            renderer: null,
            mouse:new THREE.Vector2(),
            rayCaster:new THREE.Raycaster(),
            stats:null,
            radius:6371,
            shootingS:null,
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

                gsap.to(this.shootingS.position, {duration: 20,y:-20,x:-20,ease:"Expo.easeOut"});
                
            }, 5000);
        
        },
        init: function() {
            let container = document.getElementById('container');
            //camera
            this.camera = new THREE.PerspectiveCamera(25,container.clientWidth/container.clientHeight,0.1, 1e7);
            
            //this.camera.position.z = this.radius * 5;
            this.camera.position.z = 2;

            this.scene = new THREE.Scene();
            
            //DirectionalLight
            const dirLight = new THREE.DirectionalLight( 0xffffff );
            dirLight.position.set( - 1, 0, 1 ).normalize();
            this.scene.add(dirLight);
            

            // stars
            const r = this.radius, starsGeometry = [ new THREE.BufferGeometry(), new THREE.BufferGeometry()];

            const vertices1 = [];
            const vertices2 = [];

            const vertex = new THREE.Vector3();

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

            starsGeometry[ 0 ].setAttribute( 'position', new THREE.Float32BufferAttribute( vertices1, 3 ) );
			starsGeometry[ 1 ].setAttribute( 'position', new THREE.Float32BufferAttribute( vertices2, 3 ) );

            const starsMaterials = [
                new THREE.PointsMaterial( { color: 0x555555, size: 2, sizeAttenuation: false } ),
                new THREE.PointsMaterial( { color: 0x555555, size: 1, sizeAttenuation: false } ),
                new THREE.PointsMaterial( { color: 0x333333, size: 2, sizeAttenuation: false } ),
                new THREE.PointsMaterial( { color: 0x3a3a3a, size: 1, sizeAttenuation: false } ),
                new THREE.PointsMaterial( { color: 0x1a1a1a, size: 2, sizeAttenuation: false } ),
                new THREE.PointsMaterial( { color: 0x1a1a1a, size: 1, sizeAttenuation: false } )
            ];

            for ( let i = 10; i < 30; i ++ ) {

                const stars = new THREE.Points( starsGeometry[ i % 2 ], starsMaterials[ i % 6 ] );

                stars.rotation.x = Math.random() * 6;
                stars.rotation.y = Math.random() * 6;
                stars.rotation.z = Math.random() * 6;
                stars.scale.setScalar( i * 10 );

                stars.matrixAutoUpdate = false;
                stars.updateMatrix();

                this.scene.add( stars );

            }

            //shootingS
            const sSgeometry = new THREE.ConeGeometry( 0.3, 15, 32 );
            const materialy = new THREE.MeshBasicMaterial( {color: "white"} );
            this.shootingS = new THREE.Mesh( sSgeometry, materialy );
            this.shootingS.position.set(16,16,-54)
            this.shootingS.scale.set(0.1,0.1,0.1);
            this.shootingS.rotation.set(0,0,2.2)
            this.scene.add( this.shootingS );

            //renderer
            this.renderer = new THREE.WebGLRenderer( { antialias: true } );
            this.renderer.setPixelRatio( window.devicePixelRatio );
            this.renderer.setSize(container.clientWidth, container.clientHeight);
            container.appendChild(this.renderer.domElement);

            this.controls = new OrbitControls(this.camera, this.renderer.domElement)
            this.controls.enableDamping = true;
            this.controls.dampingFactor = 0.05;
            this.controls.enableZoom = true;
            this.controls.update();

            this.stats = new Stats()
            container.appendChild( this.stats.dom );

            /*const geometry = new THREE.BoxGeometry( 1, 1, 1 );
            const material = new THREE.MeshBasicMaterial( {color: 0x00ff00} );
            const box = new THREE.Mesh( geometry, material );
            this.scene.add( box );*/
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
      this.beginTextAni();
    },
  }


</script>