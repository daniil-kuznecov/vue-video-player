<template>
  <div ref="control-block" class="control-block">
    <div class="play-control-buttons">
      <RewindBack @click="rewindVideoBackward" class="current-time-minus" />
      <Pause v-if="isVideoPlaying" @click="stopVideo" class="pause-button" />
      <Play v-else @click="playVideo" class="play-button" />
      <RewindForw @click="rewindVideoForward" class="current-time-plus" />
    </div>
    <div class="play-control-buttons">
      <VolumeOff v-if="volume === '0' || volume === 0" @click="toggleMute" class="mute-volume" />
      <Volume1 v-else-if="volume < 0.51" @click="toggleMute" />
      <Volume2 v-else @click="toggleMute" />
      <div class="volume-control">
        <input type="range" v-model="volume" :max="1" :min="0" :step="0.01" @input="setVolume" class="slider" />
      </div>
      <Shrink v-if="isFullScreen" @click="fullScreen" class="full-screen" />
      <Expand v-else @click="fullScreen" />
    </div>
  </div>
</template>

<script>
  import RewindBack from '/src/assets/icons/rewindBackwards.svg'
  import Pause from '/src/assets/icons/pause.svg'
  import Play from '/src/assets/icons/play.svg'
  import RewindForw from '/src/assets/icons/rewindForwards.svg'
  import Volume1 from '/src/assets/icons/volume-1.svg'
  import Volume2 from '/src/assets/icons/volume-2.svg'
  import VolumeOff from '/src/assets/icons/volume-off.svg'
  import Expand from '/src/assets/icons/expand.svg'
  import Shrink from '/src/assets/icons/shrink.svg'

  export default {
    components: {
      RewindBack,
      Pause,
      Play,
      RewindForw,
      Volume1,
      Volume2,
      VolumeOff,
      Expand,
      Shrink
    },
    data() {
      return {
        isFullScreen: false,
        volume: 0.5,
        isVideoPlaying: false,
        isMuted: false
      }
    },
    methods: {
      stopVideo() {
        this.$parent.$refs['video-player'].pause()
        this.isVideoPlaying = false
      },
      playVideo() {
        this.$parent.$refs['video-player'].play()
        this.isVideoPlaying = true
      },
      rewindVideoForward() {
        this.$parent.$refs['video-player'].currentTime += 10
      },
      rewindVideoBackward() {
        this.$parent.$refs['video-player'].currentTime -= 10
      },
      fullScreen() {
        const videoWrapper = this.$parent.$refs['video-wrapper']
        const controlBlock = this.$refs['control-block']
        this.isFullScreen = !this.isFullScreen
        videoWrapper.classList.toggle('fullscreen')
        controlBlock.classList.toggle('fullscreen')
      },
      setVolume() {
        this.$parent.$refs['video-player'].volume = this.volume
        document.documentElement.style.setProperty('--gradient-stop', `${this.volume * 100}%`)
      },
      toggleMute() {
        if (this.volume != 0) {
          this.volume = 0
          document.documentElement.style.setProperty('--gradient-stop', '0%')
        } else {
          this.volume = 0.5
          document.documentElement.style.setProperty('--gradient-stop', '50%')
        }
      }
    }
  }
</script>

<style>
  :root {
    --gradient-stop: 50%;
  }

  .pause-button,
  .play-button,
  .current-time-plus,
  .current-time-minus,
  .mute-volume,
  .full-screen,
  .volume-control-buttons {
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

  .control-block {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 24px 18px;
    z-index: 5 !important;
    background: white;
  }

  .control-block svg {
    stroke: #000000;
  }

  .control-block.fullscreen {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    background: #000000;
  }

  .control-block.fullscreen .slider {
    background: linear-gradient(to right, #e1e1e1 var(--gradient-stop), rgb(101, 101, 101) var(--gradient-stop));
  }

  .control-block.fullscreen .slider::-webkit-slider-thumb {
    background: #e1e1e1;
  }

  .control-block.fullscreen svg,
  .control-block.fullscreen svg path {
    fill: #e1e1e1;
    stroke: #e1e1e1;
  }

  .play-control-buttons {
    padding-left: 10px;
    padding-bottom: 2px;
    display: flex;
    align-items: center;
    gap: 16px;
  }

  .volume-control {
    display: flex;
  }
</style>
