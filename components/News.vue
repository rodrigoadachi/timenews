<template>
  <div class="shadow-xl rounded-xl bg-gray-200 carousel w-full h-full text-black">
    
    <div
      v-for="(item,key) in currentData"
      :key="item?.id"
      :id="key"
      v-show="key === currentSlide"
      class="carousel-item relative w-full" >
        
      <div :class="['image-container', { 'bg-white': !item?.urlToImage }]">
        <div v-if="item?.urlToImage" class="rounded-t-xl overflow-hidden bg-cover" :style="`background-image: url(${item?.urlToImage})`"></div>
      </div>

      <a target="_blank" :href="item?.url">
        <span class="text-xl md:text-2xl font-semibold font-sans text-[#626262]">{{ item?.title }}</span>
      

        <p class="text-xs">Author: {{ item?.author }}<span class="ml-4 text-gray-400">{{ formatDate(item?.publishedAt) }}</span></p>

        <p class="text-base font-sans mt-4 mb-4">{{ item?.description }}</p>

      </a>

      <div class="divide-y divide-blue-200 ">
        <div />
      </div>

      <div class="absolute flex justify-between transform -translate-y-1/2 left-5 right-5 top-1/2">
        
        <!-- :href="`#${currentSlide - 1}`" -->
        <div :class="`
          rounded-full
          w-10 h-10
          bg-white text-center grid place-items-center opacity-50
          ${currentSlide === 0 ?'bg-gray-400 cursor-not-allowed':'cursor-pointer'}
          `"
          @click="goTo(currentSlide - 1)">❮</div>
        
        <div @click="goTo(currentSlide + 1)"
          :class="`rounded-full w-10 h-10 bg-white text-center opacity-50 grid place-items-center
          ${currentSlide === currentData.length - 1?'bg-gray-400 cursor-not-allowed':'cursor-pointer'}
          `">❯</div>
      
      </div>

    </div>

  </div>
</template>
<script>
  export default {
    props: {
      data: { // const { data } = defineProps(['data']);
        type: Object,
      }
    },
    data() {
      return {
        currentSlide: 0,
        currentData: [],
      };
    },
    beforeMount(){
      this.currentData = this.data.filter( el => el?.title)
    },
    computed: {
      currentSlide() {
        const hash = parseInt(this.$route.hash.substring(1));
        return isNaN(hash) ? 0 : hash;
      }
    },
    methods: {
      formatDate(time) {
        const date = new Date(time);
        
        const year = date.toLocaleString("default", { year: "numeric" });
        const month = date.toLocaleString("default", { month: "2-digit" });
        const day = date.toLocaleString("default", { day: "2-digit" });
        const hour = date.toLocaleString("default", { hour: "2-digit" });
        const minute = date.toLocaleString("default", { minute: "2-digit" });
        const second = date.toLocaleString("default", { second: "2-digit" });
        
        return `${year}/${month}/${day} ${hour}:${minute}:${second}`;
      
      },
      goTo(index) {
        if (index >= 0 && index < this.currentData.length)
          this.$router.push(`/#${index}`)
      },
    },
  };

</script>

<style>
  .image-container {
    position: relative;
    width: 100%;
    height: 0;
    padding-bottom: 56.25%;
  }

  .image-container.has-image {
    padding-bottom: 0;
  }

  .image-container > div {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
</style>
