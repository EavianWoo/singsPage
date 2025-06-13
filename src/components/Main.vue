<script>
import Title from './sections/Title.vue'
import OneLine from './sections/OneLine.vue'
import Abstract from './sections/Abstract.vue'
import Markdown from './sections/Markdown.vue'
import BibTeX from './sections/BibTeX.vue'
import Video from './sections/Video.vue'
import Collapse from './sections/Collapse.vue'
import Showcases from './sections/Showcases.vue'
import Framework from './sections/Framework.vue'
import AnimVideo from './sections/AnimVideo.vue'

export default {
  components: {
    Title,
    OneLine,
    AnimVideo,
    Abstract,
    Showcases,
    Markdown,
    BibTeX,
    Video,
    Collapse,
    Framework,
  }
}
</script>

<template>
  <Title/>
<!--  <Carousel/>-->
<!--  <GsReceiver/>-->
  <OneLine/>
  <AnimVideo/>
  <Showcases/>
  <Abstract/>
  <Video/>
  <Framework/>
  <Collapse/>
  <BibTeX/>
</template>