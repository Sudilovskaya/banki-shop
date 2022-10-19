<template>
  <div class="col-lg-3 col-md-6 main__card">
    <div @click="showDialog" class="main__image">
      <img :src="post.img">
    </div>
    <div class="main__content">
      <p class="main__subtitle" @click="showDialog">{{ post.title }}</p>
      <div class="main__group">
        <div class="main__wrapper">
          <div v-if="post.sale" class="main__sale">{{ post.sale }}</div>
          <span class="main__price">{{ post.price }}</span>
        </div>
        <div class="button main__button" @click="buy">Купить</div>
      </div>
    </div>
  </div>
  <PopUp v-if="dialogVisible" v-bind:post="post" @hideDialog="hideDialog">
    <div class="wrapper">
      <swiper
          :slides-per-view="1"
          @swiper="onSwiper"
          @slideChange="onSlideChange">
        <swiper-slide>
          <div class="main__image">
            <img :src="post.slide1">
          </div>
        </swiper-slide>
        <swiper-slide>
          <div class="main__image">
            <img :src="post.slide2">
          </div>
        </swiper-slide>
      </swiper>
      <div class="popup__content">
        <p class="main__subtitle">{{ post.title }}</p>
        <p v-if="post.sale" class="main__sale">{{ post.sale }}</p>
        <p class="main__price">{{ post.price }}</p>
      </div>
    </div>
  </PopUp>
</template>

<script>
import PopUp from './PopUp';
import { Swiper, SwiperSlide } from 'swiper/vue';

import 'swiper/scss';


export default {
  props: {
    post: {
      type: Object,
      required: true,
    }
  },
  data() {
    return {
      dialogVisible: false,
    };
  },
  components: {
    PopUp,
    Swiper,
    SwiperSlide,
  },

  setup() {
    const onSwiper = (swiper) => {
      console.log(swiper);
    };
    const onSlideChange = () => {
      console.log('slide change');
    };
    return {
      onSwiper,
      onSlideChange,
    };
  },
  methods: {
    hideDialog() {
      this.dialogVisible = false;
    },
    showDialog() {
      this.dialogVisible = true;
    },
    buy() {
      this.$emit('buy');
    }
  },
};
</script>

<style lang="scss">
.wrapper {
  display: flex;
  width: 90vw;

  @media (min-width: 959px) {
    width: 500px;
  }
}

.swiper-slide {
  width: 280px;
  height: auto;

  .main__image {
    padding-bottom: 150%;
    cursor: grabbing;

    @media (min-width: 959px) {
      padding-bottom: 100%;
    }
  }
}
</style>