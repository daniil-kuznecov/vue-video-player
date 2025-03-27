<template>
  <div id="player">
    <video ref="video-player" muted="muted"></video>
  </div>
</template>

<script>
  import Hls from 'hls.js'
  export default {
    data() {
      return {
        videoSrc: 'https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.ism/.m3u8'
      }
    },
    methods: {},
    mounted() {
      if (Hls.isSupported()) {
        const video = this.$refs['video-player']
        var hls = new Hls()
        hls.loadSource(this.videoSrc)
        hls.attachMedia(video)
        hls.on(Hls.Events.MANIFEST_PARSED, function () {
          video.play()
        })
      }
    }
  }
</script>

<style>
  video {
    width: 1000px;
  }
</style>
