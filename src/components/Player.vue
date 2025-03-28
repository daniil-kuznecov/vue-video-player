<template>
  <div id="player">
    <video ref="video-player" :muted="isMuted"></video>
    <div class="volume-control">
      <input type="range" v-model="volume" :max="1" :min="0" :step="0.01" @input="setVolume" class="slider" />
    </div>
    <RotateL @click="changeCurrentTimeMinus" class="current-time-minus" />
    <Pause v-if="isVideoPlaying" @click="stopVideo" class="pause-button" />
    <Play v-else @click="playVideo" class="play-button" />
    <RotateR @click="changeCurrentTimePlus" class="current-time-plus" />
    <VolumeX v-if="volume === '0'" @click="muteVolume" class="mute-volume" />
    <Volume1 v-else-if="volume < 0.51" />
    <Volume2 v-else />
    <Expand @click="fullScreen" class="full-screen" />
  </div>
</template>

<script>
  import Hls from 'hls.js'
  import RotateL from '/src/assets/icons/rotate-L.svg'
  import Pause from '/src/assets/icons/pause.svg'
  import Play from '/src/assets/icons/play.svg'
  import RotateR from '/src/assets/icons/rotate-R.svg'
  import Volume1 from '/src/assets/icons/volume-1.svg'
  import Volume2 from '/src/assets/icons/volume-2.svg'
  import VolumeX from '/src/assets/icons/volume-x.svg'
  import Expand from '/src/assets/icons/expand.svg'

  export default {
    components: {
      RotateL,
      Pause,
      Play,
      RotateR,
      Volume1,
      Volume2,
      VolumeX,
      Expand
    },
    data() {
      return {
        videoSrc:
          'https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.ism/.m3u8',
        hls: null,
        isMuted: false,
        isFullScreen: false,
        volume: 0.5,
        isVideoPlaying: false
      }
    },
    methods: {
      stopVideo() {
        this.$refs['video-player'].pause()
        this.isVideoPlaying = false
      },
      playVideo() {
        this.$refs['video-player'].play()
        this.isVideoPlaying = true
      },
      fastForwardVideo() {
        this.$refs['video-player'].currentTime += 10
      },
      rewindVideo() {
        this.$refs['video-player'].currentTime -= 10
      },
      muteVolume() {
        this.isMuted = !this.isMuted
      },
      fullScreen() {
        const video = this.$refs['video-player']
        if (!this.isFullscreen) {
          if (video.webkitRequestFullscreen) {
            video.webkitRequestFullscreen()
          }
        } else {
          if (document.webkitExitFullscreen) {
            document.webkitExitFullscreen()
          }
        }
        this.isFullscreen = !this.isFullscreen
      },
      setVolume() {
        this.$refs['video-player'].volume = this.volume
        document.documentElement.style.setProperty('--gradient-stop', `${this.volume * 100}%`)
      }
    },
    mounted() {
      if (Hls.isSupported()) {
        const video = this.$refs['video-player']
        this.hls = new Hls()
        this.hls.loadSource(this.videoSrc)
        this.hls.attachMedia(video)
      }
    }
  }
</script>

<style>
  :root {
    --gradient-stop: 50%;
  }

  video {
    width: 1000px;
  }
  .pause-button,
  .play-button,
  .current-time-plus,
  .current-time-minus,
  .mute-volume,
  .full-screen {
    cursor: pointer;
  }

  .slider {
    border: none;
    outline: none;
    -webkit-appearance: none;
    appearance: none;
    background: linear-gradient(to right, black var(--gradient-stop), rgb(178, 178, 178) var(--gradient-stop));
    height: 5px;
    border-radius: 3px;
  }

  .slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    height: 15px;
    width: 15px;
    border-radius: 50%;
    background-color: black;
  }
</style>
