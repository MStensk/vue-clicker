<script setup>
import { computed, ref } from 'vue';
import { onMounted, onBeforeUnmount, watch } from 'vue';
import Heading from './components/Heading.Vue';
import UpgradeDesc from './components/UpgradeDesc.vue'
import { ArrowDownIcon, ArrowUpIcon } from '@heroicons/vue/24/solid'

let score = ref(0);
let addSize = ref(1);
let totalClicks = ref(0);
let perSecond = ref(0);
let clickUpgradeCost = ref(10);
let secondUpgradeCost = ref(20);
let firstUpgradeCount = ref(0);
let secondUpgradeCount = ref(0);
let thirdUpgradeCount = ref(0);
let thirdUpgradeCost = ref(100);
let fourthUpgradeCost = ref(300);
let pointsAdded = ref(0);
let isPointsAddedVisible = ref(false)
let interval = null;
let active = ref(false);
let humansRemain = ref(10000000);

const addScore = () => {
  score.value += addSize.value;
  totalClicks.value++;
  pointsAdded.value = addSize.value;
  isPointsAddedVisible.value = true;
  setTimeout(() => {
    isPointsAddedVisible.value = false;
  }, 500);
}
const increaseClick = () => {
  if (score.value >= clickUpgradeCost.value) {
    score.value -= clickUpgradeCost.value;
    addSize.value++;
    clickUpgradeCost.value = parseInt(clickUpgradeCost.value * 1.75);
  }
};

const startInterval = () => {
  interval = setInterval(() => {
    score.value += perSecond.value;
  }, 1000);
};

const firstIncrease = () => {
  if (score.value >= secondUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value++;
    score.value -= secondUpgradeCost.value;
    secondUpgradeCost.value = parseInt(secondUpgradeCost.value * 2.15);
    firstUpgradeCount.value++;
  }
  if (perSecond.value > 0) {
    startInterval();
  }
};

const secondIncrease = () => {
  if (score.value >= thirdUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value = parseInt(perSecond.value + 5);
    score.value -= thirdUpgradeCost.value;
    thirdUpgradeCost.value = parseInt(thirdUpgradeCost.value * 3);
    secondUpgradeCount.value++;
  }
  if (perSecond.value > 0) {
    startInterval();
  }
};

const thirdIncrease = () => {
  if (score.value >= fourthUpgradeCost.value) {
    clearInterval(interval);
    perSecond.value = parseInt(perSecond.value + 10);
    score.value -= fourthUpgradeCost.value;
    fourthUpgradeCost.value = parseInt(fourthUpgradeCost.value * 4.25);
    thirdUpgradeCount.value++;
    if (perSecond.value > 0) {
      startInterval();
    }
  }
};

const toggleShop = () => {
  active.value = !active.value
}

const calcHumansRemain = computed(() => {
   return humansRemain.value - score.value;
});

onMounted(() => {
  watch([score, totalClicks, addSize, perSecond, clickUpgradeCost, secondUpgradeCost, thirdUpgradeCost, fourthUpgradeCost, firstUpgradeCount, secondUpgradeCount,thirdUpgradeCount], ([newScore, newTotalClicks, newAddSize, newPerSecond, newClickUpgradeCost, newSecondUpgradeCost, newThirdUpgradeCost, newFourthUpgradeCost, newFirstUpgradeCount, newSecondUpgradeCount, newThirdUpgradeCount]) => {
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
    localStorage.setItem('thirdUpgradeCount', newThirdUpgradeCount);
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
  const savedThirdUpgradeCount = localStorage.getItem('thirdUpgradeCount');

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
  if (savedThirdUpgradeCount !== null) {
    thirdUpgradeCount.value = parseInt(savedThirdUpgradeCount);
  }
  startInterval();
});

</script>

<template>
  <Heading text="Planetoid Destroyer" />
  <main>
    <div class="stats">
      <div class="totalClicks">{{ score }} kills</div>
      <div class="perSecond"> Kills/s: {{ perSecond }}</div>
      <div class="">Humans alive: {{ calcHumansRemain }}</div>
    </div>
    <div class="grid grid-cols-2">
      <img class="planet spin m-0 col-span-2" src="https://upload.wikimedia.org/wikipedia/commons/b/b1/Ice_planet.png" width="200"
        height="200" @click="addScore()">
      <div v-if="isPointsAddedVisible" class="pointsAdded bounce w-fit">+{{ pointsAdded }} kills</div>
    </div>
    <div class="click">Click Power: {{ addSize }}</div>
    <div class="pt-4">
      <div
        class="w-64 text-center grid grid-cols-2 grid-rows-none items-center rounded-md border-2 cursor-pointer border-white border-solid bg-black"
        @click="toggleShop">
        <p class="justify-self-center ml-32">Shop
        </p>
        <ArrowDownIcon class="w-4 justify-self-end mr-2" v-if="!active" />
        <ArrowUpIcon class="w-4 justify-self-end mr-2" v-if="active" />
      </div>
      <div class="shop rounded-md border-2 border-white border-solid p-2 mt-2 bg-black absolute" v-if="active">
        <div>
          <button class="shopBuy" @click="increaseClick()">+1 Click power</button>
        </div>
        <span>Cost: {{ clickUpgradeCost }}</span>
        <div>
          <button class="shopBuy" @click="firstIncrease()">+1 Alien</button>
          <div class="upgrades">
            <img v-for="i in firstUpgradeCount" :key="i"
              src="https://www.roswell-nm.gov/images/CivicAlerts/5/AlienHead.png" width="25" height="25"
              class="upgrade-element">
              <UpgradeDesc text="Alien awards 1 kill/s" />
          </div>
          <span>Cost: {{ secondUpgradeCost }} </span>
        </div>
        <div>
          <button v-if="secondUpgradeCount >= 1" class="shopBuy" @click="secondIncrease()">+1 Gamma Gun</button>
          <button v-else @click="secondIncrease()" class="w-60">???</button>
          <div class="upgrades">
            <img v-for="i in secondUpgradeCount" :key="i" src="https://cdn-icons-png.flaticon.com/512/6754/6754301.png"
              width="25" height="25" class="upgrade-element">
              <UpgradeDesc text="Gamma gun awards 5 kills/s" />
          </div>
        </div>
        <span>Cost: {{ thirdUpgradeCost }}</span>
        <div>
          <button v-if="score >= 300" class="shopBuy" @click="thirdIncrease()">+1 Ray Gun</button>
          <button v-else class="w-60">???</button>
          <div class="upgrades">
            <img v-for="i in thirdUpgradeCount" :key="i"
              src="https://cdn-icons-png.flaticon.com/512/4681/4681020.png" width="25" height="25"
              class="upgrade-element">
              <UpgradeDesc text="Ray gun awards 10 kills/s"/>
          </div>
          <span>Cost: {{ fourthUpgradeCost }} </span>
      </div>
      </div>
    </div>
  </main>
</template>
