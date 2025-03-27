<template>
  <div id="player">
    <video ref="video-player" :muted="true"></video>
    <RotateL />
    <Pause />
    <Play />
    <RotateR />
    <Volume1 />
    <Volume2 />
    <VolumeX />
    <Expand />
    <Shrink />
  </div>
</template>

<script>
  import Hls from 'hls.js'
  import RotateL from '/src/assets/svg/rotate-L.svg'
  import Pause from '/src/assets/svg/pause.svg'
  import Play from '/src/assets/svg/play.svg'
  import RotateR from '/src/assets/svg/rotate-R.svg'
  import Volume1 from '/src/assets/svg/volume-1.svg'
  import Volume2 from '/src/assets/svg/volume-2.svg'
  import VolumeX from '/src/assets/svg/volume-x.svg'
  import Expand from '/src/assets/svg/expand.svg'
  import Shrink from '/src/assets/svg/shrink.svg'

  export default {
    components: {
      RotateL,
      Pause,
      Play,
      RotateR,
      Volume1,
      Volume2,
      VolumeX,
      Expand,
      Shrink
    },
    data() {
      return {
        videoSrc:
          'https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.ism/.m3u8',
        hls: null
      }
    },
    methods: {
      // @click="playVideo" class="play-button"
      // @click="stopVideo" class="pause-button"
      // stopVideo() {
      //   const video = this.$refs['video-player']
      //   video.pause()
      // },
      // playVideo() {
      //   const video = this.$refs['video-player']
      //   video.play()
      // }
    },
    mounted() {
      if (Hls.isSupported()) {
        const video = this.$refs['video-player']
        this.hls = new Hls()
        this.hls.loadSource(this.videoSrc)
        this.hls.attachMedia(video)
        this.hls.on(Hls.Events.MANIFEST_PARSED, function () {
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

  /* .pause-button:hover,
  .play-button:hover {
    cursor: pointer;
  } */

  /* .play-button:hover {
    cursor: pointer;
  } */
</style>
