<template>
  <div id="canvas">
  </div>
</template>

<script>
import * as three from "three"

export default {
  name: 'WebGLCanvas',
  props: {
    msg: String
  },
  mounted() {
    this.init();
    this.animate();
  },
  methods: {
    init: function() {
        let container = document.getElementById('canvas');
        this.camera = new three.PerspectiveCamera( 70, window.innerWidth / window.innerHeight, 0.01, 10 );
        this.camera.position.z = 1;
        this.scene = new three.Scene();
        this.geometry = new three.BoxGeometry( 0.2, 0.2, 0.2 );
        this.material = new three.MeshNormalMaterial();
        this.mesh = new three.Mesh( this.geometry, this.material );
        this.scene.add( this.mesh );
        this.renderer = new three.WebGLRenderer( { antialias: true } );
        this.renderer.setSize( container.clientWidth, container.clientHeight );
        container.appendChild( this.renderer.domElement );
    },
    resizeCanvasToDisplaySize: function() {
      const canvas = this.renderer.domElement;
      // look up the size the canvas is being displayed
      const width = canvas.clientWidth;
      const height = canvas.clientHeight;
      //console.log("width: " + canvas.width + ", height: " + canvas.height);
      // adjust displayBuffer size to match
      if (canvas.width !== width || canvas.height !== height) {
        // you must pass false here or three.js sadly fights the browser
        this.renderer.setSize(width, height, false);
        this.camera.aspect = width / height;
        this.camera.updateProjectionMatrix();
        // update any render target sizes here
      }
    },
    animate: function() {
        requestAnimationFrame( this.animate );
        //this.resizeCanvasToDisplaySize();
        this.mesh.rotation.x += 0.01;
        this.mesh.rotation.y += 0.02;
        this.renderer.render( this.scene, this.camera );
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
