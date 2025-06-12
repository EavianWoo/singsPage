<script>
import Title from './sections/Title.vue'
import Abstract from './sections/Abstract.vue'
import Markdown from './sections/Markdown.vue'
import BibTeX from './sections/BibTeX.vue'
import Video from './sections/Video.vue'
import Collapse from './sections/Collapse.vue'
import Showcases from './sections/Showcases.vue'
import FrameworkJS from './sections/FrameworkJS.vue'
import AnimVideo from './sections/AnimVideo.vue'

export default {
  components: {
    Title,
    AnimVideo,
    Abstract,
    Showcases,
    Markdown,
    BibTeX,
    Video,
    Collapse,
    FrameworkJS,
  }
}
</script>

<template>
  <Title/>
<!--  <Carousel/>-->
<!--  <GsReceiver/>-->
  <AnimVideo/>
  <Showcases/>
  <Abstract/>
  <Video/>
  <FrameworkJS/>
  <Collapse/>
  <BibTeX/>
</template>