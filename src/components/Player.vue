<template>
  <div class="player-wrapper">
    <div id="player">
      <ControlBar />
      <div class="progress-bar-container">
        <input
          type="range"
          min="0"
          :max="duration"
          step="1"
          v-model="currentTime"
          @input="seekToTime"
          class="progress-bar"
        />
      </div>
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
        currentTime: 0,
        duration: 0
      }
    },
    methods: {
      seekToTime() {
        const video = this.$refs['video-player']
        video.currentTime = this.currentTime
        const perc = (this.currentTime * 100) / this.duration
        document.documentElement.style.setProperty('--gradient-dur-stop', `${perc}%`)
      }
    },
    mounted() {
      if (Hls.isSupported()) {
        const video = this.$refs['video-player']
        video.addEventListener('loadeddata', () => {
          this.duration = this.$refs['video-player'].duration
        })
        video.addEventListener('timeupdate', () => {
          this.currentTime = this.$refs['video-player'].currentTime
          const perc = (this.currentTime * 100) / this.duration
          document.documentElement.style.setProperty('--gradient-dur-stop', `${perc}%`)
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
  :root {
    --gradient-dur-stop: 0%;
  }

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

  .progress-bar-container {
    width: 100%;
    height: 10px;
    position: relative;
    z-index: 2;
  }
  .progress-bar {
    width: 100%;
    border: none;
    outline: none;
    -webkit-appearance: none;
    appearance: none;
    background: linear-gradient(to right, red var(--gradient-dur-stop), rgb(178, 178, 178) var(--gradient-dur-stop));
    height: 5px;
    position: absolute;
  }

  .progress-bar::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    height: 15px;
    width: 15px;
    border-radius: 50%;
    background-color: red;
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
