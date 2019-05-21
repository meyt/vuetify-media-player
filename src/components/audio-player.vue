<template lang="pug">
  div.player-container.audio

    // Player
    div.player
      audio(ref="media")
        slot(v-if="canLoad")

    // Control Bar
    div.control-bar.visible
      scrubber(
        v-model="current"
        :min="0"
        :max="duration"
        light
        @input="seek(current)"
        :loading="isInProgress"
      )
      v-layout(align-center)
        v-flex.times
          span {{ durationTime }}
          span &nbsp;/&nbsp;
          span {{ currentTime }}
        v-spacer
        v-btn(
          icon
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
  VSpacer
} from 'vuetify/lib'
import mediaMixin from '../mixin'
import scrubber from './scrubber'

import { secondsToTime } from '../helper'

export default {
  mixins: [mediaMixin],
  components: {
    VBtn,
    VIcon,
    VLayout,
    VFlex,
    VSpacer,
    scrubber
  },
  data () {
    return {
      controlbar: false
    }
  },
  computed: {
    durationTime () {
      return secondsToTime(this.duration).join(':')
    },
    currentTime () {
      return secondsToTime(this.current).join(':')
    }
  },
  methods: {
  }
}
</script>

<style lang="stylus">
.player-container.audio
  background-color: #fff
  min-height: 80px

  .control-bar
    height: 75px

  .player
    margin-top: 24px

  .times
    margin-right: 21px
</style>
