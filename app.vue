<template>
  <div>
    <video ref="video" autoplay></video>
    <button @click="capturePhoto">Capture Photo</button>
    <canvas ref="canvas" style="display: none;"></canvas>
    <img :src="photo" v-if="photo"/>
  </div>
</template>

<script>
export default {
  data() {
    return {
      photo: null,
    };
  },
  mounted() {
    this.initCamera();
  },
  methods: {
    async initCamera() {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true });
        this.$refs.video.srcObject = stream;
      } catch (error) {
        console.error("Error accessing camera: ", error);
      }
    },
    capturePhoto() {
      const canvas = this.$refs.canvas;
      const video = this.$refs.video;
      const context = canvas.getContext('2d');
      
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      context.drawImage(video, 0, 0, canvas.width, canvas.height);
      
      this.photo = canvas.toDataURL('image/png');
    }
  }
};
</script>

<style>
video {
  width: 100%;
  height: auto;
}
</style>
