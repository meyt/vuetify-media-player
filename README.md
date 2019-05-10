# vuetify-media-player

Lightweight HTML5 video and audio player component for vuetify.


## Screenshots

### Video Player

![video player](https://github.com/meyt/vuetify-media-player/raw/master/stuff/video-player-screenshot.png?raw=true)

### Audio Player

![audio player](https://github.com/meyt/vuetify-media-player/raw/master/stuff/audio-player-screenshot.png?raw=true)

## Install

```
npm install --save vuetify-media-player
```

Currently `pug` and `stylus` webpack loaders are required.


## Usage:

```vue
<template lang="pug">
  div
    audio-player
      source(src="http://file-examples.com/wp-content/uploads/2017/11/file_example_MP3_1MG.mp3")
    video-player
      source(src="https://sample-videos.com/video123/mp4/240/big_buck_bunny_240p_10mb.mp4" type="video/mp4")
</template>

<script>
import audioPlayer from 'vuetify-media-player/src/components/audio-player.vue'
import videoPlayer from 'vuetify-media-player/src/components/video-player.vue'
import 'vuetify-media-player/src/style.styl'
export default {
  components: {
    audioPlayer,
    videoPlayer
  }
}
</script>
```
