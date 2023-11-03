
<script setup>

const props = defineProps({
  text: String,
});
</script>


<template>
    <div>
      <h1 class="md:text-4xl font-bold text-white mb-10 sm:text-3xl text-center">{{ text }}</h1>
    </div>
  </template>
