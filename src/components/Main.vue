<template>

    <div>

        <div id="container">
        </div>

    </div>

</template>

<script>

import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
import Stats from 'three/examples/js/libs/stats.min.js';
//import { gsap } from "gsap";

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
        }
    },
    methods:{
        init: function() {
            let container = document.getElementById('container');
            //camera
            this.camera = new THREE.PerspectiveCamera(25,container.clientWidth/container.clientHeight,0.1, 1e7);
            
            //this.camera.position.z = this.radius * 200;
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
                new THREE.PointsMaterial( { color: 0xffffff, size: 1, sizeAttenuation: false } ),
                new THREE.PointsMaterial( { color: 0xffffff, size: 2, sizeAttenuation: false } ),
                new THREE.PointsMaterial( { color: 0xffffff, size: 1, sizeAttenuation: false } )
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
    },
  }


</script>