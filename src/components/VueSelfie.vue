<template>
  <div>
      <h2>Vue Selfie</h2>
      <video ref="video" @canplay="initCanvas()" :width="450" :height="337.5">Stream unavaliable</video>
    
        <button @click="takePicture()">Take Picture</button>
      <canvas ref="canvas" style="display:none;"></canvas>
  </div>
</template>

<script>
export default {
  name: "VueSelfie",
  mounted() {
    this.canvas = this.$refs.canvas;
    this.video = this.$refs.video;
    this.startCapture();
  },

  data() {
    return {
      video: null,
      canvas: null,
    };
  },
  methods: {
    startCapture() {
      navigator.mediaDevices
        .getUserMedia({ video: true, audio: false })
        .then((stream) => {
          this.video.srcObject = stream;
          this.video.play();
        })
        .catch((error) => {
          console.log(error);
          alert("device is not support")
        });
    },
    takePicture() {
      let context = this.canvas.getContext("2d");
      context.drawImage(this.video,0,0,this.video.videoWidth,this.video.videoHeight);
        // context.drawImage(this.video, 0, 0, 450, 337.5);
      this.$emit("picture-taken", this.canvas.toDataURL("image/png"));
        // let can = document.getElementsByTagName("canvas");
        // let src = can[0].toDataURL("image/png");
    },
    initCanvas() {
      this.canvas.setAttribute("width", this.video.videoWidth);
      this.canvas.setAttribute("height", this.video.videoHeight);
    },
  },
};
</script>

<style>
</style>