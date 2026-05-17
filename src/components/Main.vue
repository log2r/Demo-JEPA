<script>
import Title from './sections/Title.vue'
import Abstract from './sections/Abstract.vue'
import Method from './sections/Method.vue'
import BibTeX from './sections/BibTeX.vue'
import Carousel from './sections/Carousel.vue'
import Carousel_sim from './sections/Carousel_sim.vue'
import Video from './sections/Video.vue'
import Comment from './sections/Comment.vue'
import Exp from './sections/Exp.vue'
export default {
  components: {
    Title,
    Abstract,
    Method,
    Exp,
    BibTeX,
    Carousel,
    Carousel_sim,
    Video,
  }
}
</script>

<template>
  <Title/>
  <Video/>
  <Carousel/>
  <Carousel_sim/>
  <Abstract/>
  <Method/>
  <Exp/>
  <BibTeX/>
</template>