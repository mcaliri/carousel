<template>
  <div class="carousel">
    <slot :currentSlide="currentSlide"></slot>
    <!--Navigation-->
    <div class="navigate">
      <div class="toggle-page left">
        <i @click="prevSlide" class="fa-solid fa-chevron-left"></i>
      </div>
      <div class="toggle-page right">
        <i @click="nextSlide" class="fa-solid fa-chevron-right"></i>
      </div>
    </div>
    <!--pagination-->
    <div class="pagination" v-if="paginationEnable">
      <span
        @click="goToSlide(index)"
        v-for="(slide, index) in getSlideCount"
        :key="index"
        :class="{ active: index + 1 === currentSlide }"
      >
      </span>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "CarouselComponent",
  props: ["startAutoplay", "navigation", "pagination", "timeoutDuration"],
  setup(props) {
    const currentSlide = ref(1);
    const getSlideCount = ref(null);

    const autoPlayEnabled = ref(
      props.startAutoplay === undefined ? true : props.startAutoplay
    );
    const paginationEnable = ref(
      props.pagination === undefined ? true : props.pagination
    );
    const navigationEnable = ref(
      props.navigation === undefined ? true : props.navigation
    );
    const timeoutDuration = ref(
      props.timeoutDuration === undefined ? 5000 : props.timeoutDuration
    );

    //next slide
    const nextSlide = () => {
      currentSlide.value++;
      if (currentSlide.value > getSlideCount.value) {
        currentSlide.value = 1;
      }
    };

    //previous slide
    const prevSlide = () => {
      currentSlide.value--;
      if (currentSlide.value < 1) {
        currentSlide.value = getSlideCount.value;
      }
    };

    const goToSlide = (index) => {
      currentSlide.value = index + 1;
    };

    // autoplay
    const autoPlay = () => {
      setInterval(() => {
        nextSlide();
      }, timeoutDuration.value);
    };

    if (autoPlayEnabled.value) {
      autoPlay();
    }

    onMounted(() => {
      getSlideCount.value = document.querySelectorAll(".slide").length;
    });

    return {
      currentSlide,
      getSlideCount,
      nextSlide,
      prevSlide,
      goToSlide,
      autoPlay,
      autoPlayEnabled,
      paginationEnable,
      navigationEnable,
    };
  },
};
</script>
<style scoped>
.navigate {
  padding: 0 16px;
  height: 100%;
  width: 100%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
}

.toggle-page {
  display: flex;
  flex: 1;
}
.right {
  justify-content: flex-end;
}
i {
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  background-color: #6347c7;
  color: #fff;
}
.pagination {
  position: absolute;
  bottom: 24px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 16px 0;
  gap: 16px;
}
span {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background-color: #fff;
  cursor: pointer;
}
.active {
  background-color: #6347c7;
}
</style>
