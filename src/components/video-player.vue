<template lang="pug">
  div(
    ref="mediaPlayer"
    @mouseover="onMouseHover"
    @dblclick.prevent="() => doubleClickFullscreen ? toggleFullScreen() : null"
    :class="classes"
  )
    // Player
    div.player(@click.prevent="unmuteOrTogglePlay")
      video(ref="media" :width="width" :height="height")
        slot(v-if="canLoad")

    // Pre-loader
    v-progress-circular.preloader(
      @click="unmuteOrTogglePlay()"
      v-if="isInProgress"
      indeterminate
      size="64"
    )

    // Control Bar
    div(:class="{'control-bar': true, 'visible': controlbar || paused}")
      scrubber(
        v-model="current"
        :min="0"
        :max="duration"
        @input="seek(current)"
        :loading="isInProgress"
      )
      v-layout(align-center)
        v-btn(
          icon
          dark
          large
          @click="toggleFullScreen()"
        )
          v-icon(large) {{ fullscreen ? 'fullscreen_exit' : 'fullscreen' }}
        v-btn(
          icon
          dark
          large
          @click="toggleMute()"
        )
          v-icon(large) {{ muted ? 'volume_off' : 'volume_up' }}
        v-flex.times
          span {{ durationTime }}
          span &nbsp;/&nbsp;
          span {{ currentTime }}
        v-spacer
        v-btn(
          icon
          dark
          large
          @click="unmuteAndTogglePlay()"
        )
          v-icon(large) {{ playing ? 'pause' : ended ? 'replay' : 'play_arrow' }}
</template>

<script>
import {
  VBtn,
  VIcon,
  VLayout,
  VFlex,
  VSpacer,
  VProgressCircular
} from 'vuetify/lib'
import scrubber from './scrubber'
import mediaMixin from '../mixin'
import { secondsToTime } from '../helper'

export default {
  mixins: [mediaMixin],
  props: {
    width: {
      type: Number,
      default: null
    },
    height: {
      type: Number,
      default: null
    },
    doubleClickFullscreen: {
      type: Boolean,
      default: false
    }
  },
  components: {
    VBtn,
    VIcon,
    VLayout,
    VFlex,
    VSpacer,
    VProgressCircular,
    scrubber
  },
  data () {
    return {
      fullscreen: false,
      controlbar: false
    }
  },
  computed: {
    classes () {
      return {
        'player-container video': true,
        'fullscreen': this.fullscreen
      }
    },
    durationTime () {
      return secondsToTime(this.duration).join(':')
    },
    currentTime () {
      return secondsToTime(this.current).join(':')
    }
  },
  methods: {
    onMouseHover () {
      this.controlbar = true
      window.clearTimeout(this.__controlbarTimeout)
      this.__controlbarTimeout = window.setTimeout(() => {
        this.controlbar = false
      }, 3000)
    },
    exitFullScreen () {
      if (document.exitFullscreen) {
        document.exitFullscreen()
      } else if (document.mozCancelFullScreen) {
        document.mozCancelFullScreen()
      } else if (document.webkitExitFullscreen) {
        document.webkitExitFullscreen()
      } else if (document.msExitFullscreen) {
        document.msExitFullscreen()
      }
      this.fullscreen = false
    },
    enterFullScreen () {
      const el = this.$refs.mediaPlayer
      if (el.requestFullscreen) {
        el.requestFullscreen()
      } else if (el.mozRequestFullScreen) {
        el.mozRequestFullScreen()
      } else if (el.webkitRequestFullscreen) {
        el.webkitRequestFullscreen()
      } else if (el.msRequestFullscreen) {
        el.msRequestFullscreen()
      }
      this.fullscreen = true
    },
    toggleFullScreen () {
      if (this.fullscreen) {
        this.exitFullScreen()
      } else {
        this.enterFullScreen()
      }
    }
  }
}
</script>

<style lang="stylus">
.player-container.video
  color: rgba(255, 255, 255, 0.85)

  video
    display: block
    width: 100%
    height: 100%

  .control-bar
    background: linear-gradient(rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.65))

  .preloader
    position: absolute
    top: 50%
    left: 50%
    margin-left: -(64 / 2)px
    margin-top: -(64 / 2)px
</style>
