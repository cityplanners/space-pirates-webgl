<template>
  <button @click="sendRequest">Click me!</button>
  <div id="canvas">
  </div>
</template>

<script>
import * as three from "three"
import { FirstPersonControls } from './controls/FirstPersonControls.js'

let controls;
const clock = new three.Clock();

export default {
  name: 'WebGLCanvas',
  props: {
    msg: String
  },
  data() {
    return {
      boxDimensions: {
        x: 0.2,
        y: 0.2,
        z: 0.2
      }
    }
  },
  mounted() {
    this.init();
    this.animate();
  },
  methods: {
    init: function() {
        let container = document.getElementById('canvas');
        this.camera = new three.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 0.001, 100 );
        this.camera.position.z = 1;

        this.camera.position.set( 0, 0, 2 );
				this.camera.lookAt( 0, 0, 0 );

        this.scene = new three.Scene();
        console.log(this.boxDimensions.x + " " + this.boxDimensions.y + " " + this.boxDimensions.z );
        this.geometry = new three.BoxGeometry( this.boxDimensions.x, this.boxDimensions.y, this.boxDimensions.z );
        this.material = new three.MeshNormalMaterial();
        this.mesh = new three.Mesh( this.geometry, this.material );
        this.scene.add( this.mesh );
        this.renderer = new three.WebGLRenderer( { antialias: true } );
        //this.renderer.setSize( container.clientWidth, container.clientHeight );
        this.renderer.setSize( window.innerWidth, window.innerHeight );

        // Attach movement controls to camera
        controls = new FirstPersonControls( this.camera, this.renderer.domElement );
				controls.movementSpeed = .1;
				controls.lookSpeed = 0.2;

        container.appendChild( this.renderer.domElement );
    },
    resizeCanvasToDisplaySize: function() {
      const canvas = this.renderer.domElement;
      // look up the size the canvas is being displayed
      const width = canvas.clientWidth;
      const height = canvas.clientHeight;
      // adjust displayBuffer size to match
      if (canvas.width !== width || canvas.height !== height) {
        // you must pass false here or three.js sadly fights the browser
        this.renderer.setSize(width, height, false);
        this.camera.aspect = width / height;
        this.camera.updateProjectionMatrix();
        // update any render target sizes here
      }

      controls.handleResize();
    },
    render: function() {
        controls.update( clock.getDelta() );
        this.renderer.render( this.scene, this.camera );
    },
    animate: function() {
        requestAnimationFrame( this.animate );
        
        // Update
        //this.resizeCanvasToDisplaySize();
        this.mesh.rotation.x += 0.01;
        this.mesh.rotation.y += 0.02;
        // update box size or whatever (PoC)
        // this.mesh.geometry.parameters.depth = this.boxDimensions.z;
        // this.mesh.geometry.parameters.width = this.boxDimensions.x;
        // this.mesh.geometry.parameters.height = this.boxDimensions.y;

        this.render();
    },
    sendRequest() {
      console.log('Click!');
      // Request box dimensions from server
      // Simple POST request with a JSON body using fetch
      const requestOptions = {
        method: "GET",
        mode: 'cors',
        headers: { "Content-Type": "application/json" }
      };
      fetch("http://localhost:8080/cubeDimensions", requestOptions)
        .then(response => response.json())
        .then(data => {
          console.log('assign data: ' + data);
          this.boxDimensions = data.dimensions;
        });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#canvas {
  width:100%;
  height:100%;
}
</style>
