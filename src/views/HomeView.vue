<template>
  <div class="home">
    <Carousel
        :navigation="true"
        :pagination="true"
        :startAutoPlay="true"
        :timeout="5000"
        class="carousel"
        v-slot="{currentSlide}"
    >
      <Slide v-for="(slide, index) in carouselSlides" :key="index">
        <div v-show="currentSlide === index + 1" class="slide-info">
          <img :src="require(`../assets/${slide}.jpg`)">
        </div>
      </Slide>
    </Carousel>
  </div>
</template>

<script>
import Carousel from "@/components/Carousel";
import Slide from "@/components/Slide";

export default {
  name: 'HomeView',
  components: {
    Carousel,
    Slide
  },
   setup() {
    const carouselSlides = ['img-1', 'img-2', 'img-3'];

    return { carouselSlides };
   }
}
</script>

<style lang="scss" scoped>

.carousel {
  position: relative;
  max-height: 100vh;
  height: 100vh;

  .slide-info {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    max-height: 100%;
    height: 100%;

    img {
      min-width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
}
</style>
