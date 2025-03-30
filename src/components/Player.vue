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
        <ul class="video-ul">
          <li
            v-for="(name, index) in videoNames"
            @click="setVideo(index)"
            :class="{ 'selected-video': selectedIndex === index }"
          >
            {{ name }}
          </li>
        </ul>
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
        duration: 0,
        videoPlaying: null,
        selectedIndex: 0,
        videoNames: ['Видео #1', 'Видео #2', 'Видео #3'],
        links: [
          'https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.ism/.m3u8',
          'https://devstreaming-cdn.apple.com/videos/streaming/examples/img_bipbop_adv_example_ts/master.m3u8',
          'https://demo.unified-streaming.com/k8s/features/stable/video/tears-of-steel/tears-of-steel.ism/.m3u8'
        ]
      }
    },
    methods: {
      setVideo(index) {
        this.videoSrc = this.links[index]
        this.selectedIndex = index
        const video = this.$refs['video-player']
        this.hls.loadSource(this.videoSrc)
      }
    },
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
      this.videoPlayer = this.$refs['video-player']
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
    align-items: flex-start;
    text-align: center;
    justify-content: flex-start;
    padding-top: 20px;
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

  .video-list {
    margin-top: 50px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
  }

  .video-list li {
    padding: 6px 12px;
    text-align: left;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.1s;
  }

  .video-list li:hover {
    background-color: #e1e1e1;
  }

  .video-list li.selected {
    border: 1px solid black;
    font-family: 'PlayBold';
  }

  .video-ul {
    list-style-type: none;
  }

  .selected-video {
    font-weight: bold;
    border: 1px solid black;
  }
</style>
