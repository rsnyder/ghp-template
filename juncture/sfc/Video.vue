<template>

  <div :style="containerStyle">
    <div class="plyr__video-embed" id="player">
      <iframe
        :src="`${source}`"
        allowfullscreen
        allowtransparency
        allow="autoplay"
        :width="playerWidth - 12"
      ></iframe>
    </div>
  </div>

</template>

<script>

const viewerLabel = 'Video Viewer'
const viewerIcon = 'fas fa-video'
const dependencies = [
  'https://cdnjs.cloudflare.com/ajax/libs/plyr/3.6.7/plyr.min.js',
  'https://cdn.plyr.io/3.6.7/plyr.css'
  ]

module.exports = {
  name: 've-video',
  props: {
    items: { type: Array, default: () => ([]) },
    viewerIsActive: Boolean
  },
  data: () => ({
    viewerLabel,
    viewerIcon,
    dependencies,
    playerVars: {
      ytppauseoverlay: 0,
      modestbranding: 1,
      rel: 0,
      showinfo: 0,
      autohide: 1,
      playsinline: 1
    },
    isPlaying: false,
    playerWidth: 564
  }),
  computed: {
    videoItems() { return this.items.filter(item => item[this.$options.name]) },
    videoId() { return this.videoItems.length > 0 ? this.videoItems[0].vid || this.videoItems[0].id : null },
    videoSource() {return this.videoItems.length > 0 ? this.videoItems[0].source : null },
    source() { return this.videoSource ? this.videoSource : 'https://www.youtube.com/embed/'+this.videoId},
    containerStyle() { return { display: this.viewerIsActive ? '' : 'none', height: this.viewerIsActive ? '100%' : '0' } }
  },
  mounted() {
    this.loadDependencies(dependencies, 0, this.init)
  },
  /*
  beforeDestroy() {
    this.isPlaying = false
    if (this.isPlaying) {
      this.player.stopVideo()
    }
  },
  */
  methods: {
    init() {

    },
    playVideo() {
      this.player.playVideo()
    },
    playing() {
      this.isPlaying = true
    },
    paused() {
      this.isPlaying = false
    },
    ready() {
      // console.log('Video player ready')
    }
  }
}
</script>

<style>
  .youtube-iframe {
    position: absolute;
    margin-top: 64px;
  }
</style>
