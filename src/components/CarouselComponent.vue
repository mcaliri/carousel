<template>
  <div class="carousel">
    <slot :currentSlide="currentSlide"></slot>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "CarouselComponent",
  setup() {
    const currentSlide = ref(1);
    return { currentSlide};
  },
};


</script>