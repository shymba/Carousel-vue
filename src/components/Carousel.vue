<template>
  <div class="carousel">
    <slot :currentSlide="currentSlide"/>

    <div v-if="navEnabled" class="navigate">
      <div class="toggle-page left">
        <i @click="prevSlide" class="fas fa-chevron-left"></i>
      </div>
      <div class="toggle-page right">
        <i @click="nextSlide" class="fas fa-chevron-right"></i>
      </div>
    </div>

    <div v-if="paginationEnabled" class="pagination">
      <span
          @click="goToSlide(index)"
          v-for="(slide, index) in getSlideCount"
          :key="index"
          :class="{active : index + 1 === currentSlide}"
      >
      </span>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  name: "Carousel",
  props: ['startAutoPlay', 'timeout', 'navigation', 'pagination'],
  setup(props) {
    const currentSlide = ref(1);
    const getSlideCount = ref(null);
    const autoPlayEnable = ref(
        props.startAutoPlay === undefined ? true : props.startAutoPlay
    );
    const timeOutDuration = ref(
        props.timeout === undefined ? 5000 : props.timeout
    );
    const paginationEnabled = ref(
        props.pagination === undefined ? true : props.pagination
    );
    const navEnabled = ref(
        props.navigation === undefined ? true : props.navigation
    );

    const nextSlide = () => {
      if(currentSlide.value === getSlideCount.value) {
        currentSlide.value = 1;
        return;
      }
      currentSlide.value +=1;
    };

    const prevSlide = () => {
      if(currentSlide.value === 1) {
        currentSlide.value = 1;
        return;
      }
      currentSlide.value -= 1
    };

    const goToSlide = (index) => {
      currentSlide.value = index + 1;
    };

    const autoPlay = () => {
      setInterval(() => {
        nextSlide()
      }, timeOutDuration.value)
    };

    if(autoPlayEnable.value) {
      autoPlay();
    }

    onMounted(() => {
      getSlideCount.value = document.querySelectorAll('.slide').length;
    })

    return {
      currentSlide,
      nextSlide,
      prevSlide,
      getSlideCount,
      goToSlide,
      paginationEnabled,
      navEnabled
    }
  }
}
</script>

<style lang="scss">

.navigate {
  padding: 0 16px;
  height: 100%;
  width: 100%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;

  .toggle-page {
    display: flex;
    flex: 1;
  }

  .right {
    justify-content: flex-end;
  }

  i {
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    border-radius: 50%;
    width: 30px;
    height: 30px;
    background-color: #2F2057;
    color: #ffffff;
  }
}

.pagination {
  position: absolute;
  bottom: 24px;
  width: 100%;
  display: flex;
  gap: 16px;
  justify-content: center;
  align-items: center;

  span {
    cursor: pointer;
    width: 20px;
    height: 20px;
    border-radius: 50px;
    background-color: #ffffff;
  }

  .active {
    background-color: #7a5dce;
  }
}

</style>