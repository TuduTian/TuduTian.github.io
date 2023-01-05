

##  vu3 wiper8.x版本 autoplay 失效的问题
> swiper官网:https://swiperjs.com/vue

```vue
<template>
      <swiper @swiper="onSwiper" ref="swiperDom" :effect="'fade'" :modules="modules" :autoplay="autoplayOptions"
        :loop="true">
        <swiper-slide>
          <img :src="getAssetsImages('banner.jpg')" alt="">
        </swiper-slide>
        <swiper-slide>
          <img :src="getAssetsImages('banner1.jpg')" alt="">
        </swiper-slide>
      </swiper>
</template>

<script setup>
  import { Swiper, SwiperSlide, } from 'swiper/vue';
  import { Autoplay, EffectFade } from 'swiper'
  import "swiper/css/effect-fade"; // 这个是动画 可要可不要
  import 'swiper/css/bundle'
  const swiperDom = ref(null)
  const onSwiper = (swiper) => {
    setTimeout(() => {
      swiper.autoplay.start() //最重要的是这一点 要进行start一下不然的话可能会不进行轮播
    }, 100)
  }
  // swiper modules 
  const modules = [Autoplay, EffectFade]
  //自动轮播的配置
  const autoplayOptions = {
    delay: 2600,
    disableOnInteraction: false,
  }
</script>
```