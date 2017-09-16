<template>
  <div class="swiper-container">
    <div class="swiper-wrapper">
      <slot></slot>
    </div>
    <div class="swiper-pagination" v-if="paginationComputed === true"></div>
    <div class="swiper-scrollbar" v-if="scrollbarComputed === true"></div>
    <div class="swiper-button-next" v-if="navigationComputed === true"></div>
    <div class="swiper-button-prev" v-if="navigationComputed === true"></div>
  </div>
</template>
<script>
  import Utils from '../utils/utils';

  let initialUpdate = false;

  export default {
    beforeDestroy() {
      const self = this;
      if (!self.init) return;
      if (self.swiper && self.swiper.destroy) self.swiper.destroy();
    },
    updated() {
      const self = this;
      if (!initialUpdate) {
        initialUpdate = true;
        return;
      }
      if (self.swiper && self.swiper.update) self.swiper.update();
    },
    props: {
      params: Object,
      pagination: Boolean,
      scrollbar: Boolean,
      navigation: Boolean,
      init: {
        type: Boolean,
        default: true,
      },
    },
    computed: {
      paginationComputed() {
        const self = this;
        if (self.pagination === true || (self.params && self.params.pagination && !self.params.pagination.el)) {
          return true;
        }
        return false;
      },
      scrollbarComputed() {
        const self = this;
        if (self.scrollbar === true || (self.params && self.params.scrollbar && !self.params.scrollbar.el)) {
          return true;
        }
        return false;
      },
      navigationComputed() {
        const self = this;
        if (self.navigation === true || (self.params && self.params.navigation && !self.params.navigation.nextEl && !self.params.navigation.prevEl)) {
          return true;
        }
        return false;
      },
    },
    methods: {
      onF7Ready(f7) {
        const self = this;
        if (!self.init) return;
        const params = {
          pagination: {},
          navigation: {},
          scrollbar: {},
        };
        if (self.params) Utils.extend(params, self.params);
        if (self.pagination && !params.pagination.el) params.pagination.el = '.swiper-pagination';
        if (self.navigation && !params.navigation.nextEl && !params.navigation.prevEl) {
          params.navigation.nextEl = '.swiper-button-next';
          params.navigation.prevEl = '.swiper-button-prev';
        }
        if (self.scrollbar && !params.scrollbar.el) params.scrollbar.el = '.swiper-scrollbar';

        self.swiper = f7.swiper.create(this.$el, params);
      },
    },
  };
</script>