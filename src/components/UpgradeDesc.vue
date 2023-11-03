
<script setup>

const props = defineProps({
  text: String,
});
</script>


<template>
    <div>
      <p class="text-gray-700 text-xs">{{ text }}</p>
    </div>
  </template>
