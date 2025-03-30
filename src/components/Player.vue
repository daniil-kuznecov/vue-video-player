<template>
  <div class="player-wrapper">
    <div id="player">
      <ControlBar :video-duration="duration" />
      <div ref="video-wrapper" class="video-wrapper">
        <video ref="video-player" :muted="isMuted" />
      </div>
    </div>
    <div class="available-video">
      <h3>Доступные видео</h3>
      <div class="video-list">
        <p>Видео #1</p>
        <p>Видео #2</p>
        <p>Видео #3</p>
      </div>
    </div>
  </div>
</template>

<script>
  import Hls from 'hls.js'
  import ControlBar from './ControlBar.vue'

  export default {
    components: {
      ControlBar
    },
    data() {
      return {
        videoSrc:
          'https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.ism/.m3u8',
        hls: null,
        isMuted: false,
        duration: 0
      }
    },
    methods: {},
    mounted() {
      if (Hls.isSupported()) {
        const video = this.$refs['video-player']
        video.addEventListener('loadeddata', () => {
          this.duration = this.$refs['video-player'].duration
        })
        this.hls = new Hls()
        this.hls.loadSource(this.videoSrc)
        this.hls.attachMedia(video)
      }
      this.videoPlayer = this.$refs.videoPlayer
    }
  }
</script>

<style>
  video {
    width: 100%;
    z-index: 1 !important;
  }

  video::-webkit-media-controls {
    display: none !important;
  }

  #player {
    width: 70%;
    border-radius: 12px;
    overflow: hidden;
    background-color: rgb(255, 255, 255);
    display: flex;
    flex-direction: column-reverse;
    box-shadow: var(--box-shadow);
  }

  .player-wrapper {
    display: flex;
    justify-content: space-between;
  }

  .available-video {
    width: 20%;
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    justify-content: flex-start;
    padding-top: 20px;
  }

  .video-list {
    margin-top: 50px;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  .video-wrapper {
    z-index: 4;
  }

  .video-wrapper.fullscreen {
    position: absolute;
    width: 100vw;
    left: 0;
    top: 0;
    background: black;
    height: 100vh;
  }

  .video-wrapper.fullscreen video {
    width: 100%;
    height: auto;
    transform: translateY(-50%);
    position: absolute;
    top: 50%;
  }
</style>
