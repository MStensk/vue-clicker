<script setup>
import { ref } from 'vue';
import { onMounted, onBeforeUnmount, watch } from 'vue';
let score = ref(0);
let addSize = ref(1);
let totalClicks = ref(0);
let perSecond = ref(0);
let clickUpgradeCost = ref(10);
let secondUpgradeCost = ref(20);
let firstUpgradeCount = ref(0);
let secondUpgradeCount = ref(0);
let thirdUpgradeCost = ref(100);
let fourthUpgradeCost = ref(300);
let pointsAdded = ref(0);
let isPointsAddedVisible = ref(false)
let interval = null;

function addScore() {
  score.value += addSize.value;
  totalClicks.value++;
  pointsAdded.value = addSize.value;
  isPointsAddedVisible.value = true;
  setTimeout(() => {
    isPointsAddedVisible.value = false;
  }, 500);
}
function increaseClick() {
  if (score.value >= clickUpgradeCost.value) {
    score.value -= clickUpgradeCost.value;
    addSize.value++;
    clickUpgradeCost.value = parseInt(clickUpgradeCost.value * 1.75)
  }
}
function startInterval() {
  interval = setInterval(() => {
    score.value += perSecond.value;
  },1000)
}
function firstIncrease() {
  if (score.value >= secondUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value++;
    score.value -= secondUpgradeCost.value;
    secondUpgradeCost.value = parseInt(secondUpgradeCost.value * 2.15)
    firstUpgradeCount.value++
  }
  if(perSecond.value > 0) {
    startInterval();
  }
}
function secondIncrease() {
  if(score.value >= thirdUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value = parseInt(perSecond.value + 5);
    score.value -= thirdUpgradeCost.value;
    thirdUpgradeCost.value = parseInt(thirdUpgradeCost.value * 3)
    secondUpgradeCount.value++;
  }
  if(perSecond.value > 0) {
    startInterval();
  }
}
function thirdIncrease() {
  if(score.value >= fourthUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value = parseInt(perSecond.value + 10);
    score.value -= fourthUpgradeCost.value;
    fourthUpgradeCost.value = parseInt(fourthUpgradeCost.value * 4.25);
    if(perSecond.value > 0) {
    startInterval();
  }
  }
}

onMounted(() => {
  watch([score, totalClicks, addSize, perSecond,clickUpgradeCost,secondUpgradeCost, thirdUpgradeCost, fourthUpgradeCost, firstUpgradeCount, secondUpgradeCount], ([newScore, newTotalClicks, newAddSize, newPerSecond,newClickUpgradeCost,newSecondUpgradeCost, newThirdUpgradeCost, newFourthUpgradeCost, newFirstUpgradeCount, newSecondUpgradeCount]) => {
    localStorage.setItem('score', newScore);
    localStorage.setItem('totalClicks', newTotalClicks);
    localStorage.setItem('addSize', newAddSize);
    localStorage.setItem('perSecond', newPerSecond);
    localStorage.setItem('clickUpgradeCost', newClickUpgradeCost);
    localStorage.setItem('secondUpgradeCost', newSecondUpgradeCost);
    localStorage.setItem('thirdUpgradeCost', newThirdUpgradeCost);
    localStorage.setItem('fourthUpgradeCost', newFourthUpgradeCost);
    localStorage.setItem('firstUpgradeCount', newFirstUpgradeCount);
    localStorage.setItem('secondUpgradeCount', newSecondUpgradeCount);
  });

  // Load data from localStorage and update the variables
  const savedScore = localStorage.getItem('score');
  const savedTotalClicks = localStorage.getItem('totalClicks');
  const savedAddSize = localStorage.getItem('addSize');
  const savedPerSecond = localStorage.getItem('perSecond');
  const savedClickUpgrade = localStorage.getItem('clickUpgradeCost');
  const savedSecondUpgrade = localStorage.getItem('secondUpgradeCost');
  const savedThirdUpgrade = localStorage.getItem('thirdUpgradeCost');
  const savedFourthUpgrade = localStorage.getItem('fourthUpgradeCost');
  const savedFirstUpgradeCount = localStorage.getItem('firstUpgradeCount');
  const savedSecondUpgradeCount = localStorage.getItem('secondUpgradeCount');

  if (savedScore !== null) {
    score.value = parseInt(savedScore);
  }

  if (savedTotalClicks !== null) {
    totalClicks.value = parseInt(savedTotalClicks);
  }

  if (savedAddSize !== null) {
    addSize.value = parseInt(savedAddSize);
  }

  if (savedClickUpgrade !== null) {
    clickUpgradeCost.value = parseInt(savedClickUpgrade);
  }
  if (savedSecondUpgrade !== null) {
    secondUpgradeCost.value = parseInt(savedSecondUpgrade);
  }
  if (savedThirdUpgrade !== null) {
    thirdUpgradeCost.value = parseInt(savedThirdUpgrade);
  }
  if (savedFourthUpgrade !== null) {
    fourthUpgradeCost.value = parseInt(savedFourthUpgrade);
  }
  if (savedPerSecond !== null) {
    perSecond.value = parseInt(savedPerSecond);
  }
  if (savedFirstUpgradeCount !== null) {
    firstUpgradeCount.value = parseInt(savedFirstUpgradeCount);
  }
  if (savedSecondUpgradeCount !== null) {
    secondUpgradeCount.value = parseInt(savedSecondUpgradeCount);
  }
  startInterval();
});
</script>

<template>
  <header>
  </header>

  <main>
<div class="stats">
<div class="totalClicks">{{ score }}</div>  
<div class="perSecond"> Kills/s: {{ perSecond }}</div>
</div>
<img class="planet" src="https://upload.wikimedia.org/wikipedia/commons/b/b1/Ice_planet.png" width="200" height="200" @click="addScore()">
<div v-if="isPointsAddedVisible" class="pointsAdded bounce">+{{ pointsAdded }}</div>
<div class="click">Click Power: {{ addSize }}</div>
<div class="shop">
  
<div>
<span>Cost: {{ clickUpgradeCost }}</span>
<button class="shopBuy" @click="increaseClick()">+1 Click power</button>
</div>
<div>
<span>Cost: {{ secondUpgradeCost }} </span>
<button class="shopBuy" @click="firstIncrease()">+1 Alien</button>  
<div class="upgrades">
<img v-for="i in firstUpgradeCount"
 :key="i"
 src="https://www.roswell-nm.gov/images/CivicAlerts/5/AlienHead.png"
 width="25"
 height="25"
  class="upgrade-element">
</div>
</div>
<div>
<span>Cost: {{ thirdUpgradeCost }}</span>
<button v-if="secondUpgradeCount >= 1 " class="shopBuy" @click="secondIncrease()">+1 Gamma Gun</button>
<button v-else @click="secondIncrease()" class="">???</button>
<div class="upgrades">
<img v-for="i in secondUpgradeCount"
 :key="i"
 src="https://cdn-icons-png.flaticon.com/512/6754/6754301.png"
 width="25"
 height="25"
  class="upgrade-element">
</div>
</div>
<div>
<span>Cost: {{ fourthUpgradeCost }}</span>
<button v-if="score >= 300" class="shopBuy" @click="thirdIncrease()">+10 kills pr. second</button>
<button v-else class="">???</button>
</div>
</div>
  </main>
</template>
