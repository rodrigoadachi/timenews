<template>
  <div class="px-5 sm:px-60 py-4">

    <p class="text-3xl font-bold text-red-600">Time News</p>

    <News :data="data?.articles" class="px-4 py-4 w-1/2 mt-5"/>

  </div>
</template>

<script setup>
  const config = useRuntimeConfig()
  console.log('Runtime config:', config)
  const url = `${config.public.apiBase}/top-headlines?country=us&apiKey=${config.public.apiSecret}`
  const { data } = await useFetch(url)
</script>