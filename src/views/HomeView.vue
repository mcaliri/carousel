<template>
  <div class="home">
    <CarouselComponent class="carousel" v-slot="{ currentSlide }">
      <SlideComponent v-for="(slide, index) in carouselSlider" :key="index">
        <div v-show="currentSlide === index + 1" class="slide-info">
            <img :src="require(`../assets/images/${slide}.jpg`)" alt="slide" />
        </div>
    </SlideComponent>
    </CarouselComponent>
  </div>
</template>

<script>
import SlideComponent from "../components/SlideComponent.vue";
import CarouselComponent from "../components/CarouselComponent.vue";

export default {
  name: "HomeView",
  components: {
    SlideComponent,
    CarouselComponent,
  },
  setup() {
    const carouselSlider = ["bg-1", "bg-2", "bg-3"];
    return {
      carouselSlider,
    };
  },
};
</script>
<style scoped>
.carousel {
    position: relative;
  max-height: 100vh;
  height: 100vh;
}
.slide-info{
    position: absolute;
   top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    max-height: 100%;
  }
  img{
    min-width: 100%;
    height: 100%;
    object-fit: cover;
  }
</style>
