<template>
    <div class="income-upgrade">
      <!-- Your component's template here -->
      <button @click="firstIncrease()">Upgrade Income</button>
      <span>{{ increaseLevel }}</span>
    </div>
  </template>
  
  <script>
  export default {
    props: ['increaseLevel'],
    methods: {
      firstIncrease() {
        if (score.value >= secondUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value++;
    score.value -= secondUpgradeCost.value;
    secondUpgradeCost.value = parseInt(secondUpgradeCost.value * 2.15)
  }
  if(perSecond.value > 0) {
    startInterval();
  }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Your component's styles here */
  </style>