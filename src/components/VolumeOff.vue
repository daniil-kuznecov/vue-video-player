<template>
  <VolumeOff v-if="volume === '0'" @click="muteVolume" class="mute-volume" />
  <Volume1 v-else-if="volume < 0.51" @click="toggleMute" />
  <Volume2 v-else @click="toggleMute" />

  <Volume1 v-if="volume" />
</template>

<script>
  export default {
    data() {
      return {
        volume: 0.5,
        latestVolume: 0.5,
        isMuted: false
      }
    },
    methods: {
      toggleMute() {
        if (this.volume > 0) {
          this.isMuted = !this.isMuted
          this.latestVolume = this.volume
          document.documentElement.style.setProperty('--gradient-stop', '0%')
        } else {
          this.volume = this.latestVolume
          document.documentElement.style.setProperty('--gradient-stop', `${this.volume * 100}%`)
        }
      }
    }
  }
</script>

<style></style>
<!-- if (!this.isMuted) {
  //   this.latestVolume = this.volume
  //   this.volume = 0
  // } else {
  //   this.volume = this.latestVolume
  // }
  // this.isMuted = !this.isMuted
  // if (this.$refs['video-player']) {
  //   this.$refs['video-player'].muted = this.isMuted
  // } -->
