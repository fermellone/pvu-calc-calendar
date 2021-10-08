<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import { ref, computed } from "vue";

// Constants
import {
  initialSapling,
  initialMama,
  worldTreeIncomes as worldTree,
} from "./helpers/constants";

// Helpers
import uuidGen from "./helpers/uuidGenerator";

// Components
import FarmItemGenerator from "./components/FarmItemGenerator.vue";
import Plant from "./components/Plant.vue";
import Sapling from "./components/Sapling.vue";
import Mama from "./components/Mama.vue";
import CalendarTable from "./components/CalendarTable.vue";

// Data
const plants = ref([]);
const saplings = ref([]);
const mamas = ref([]);
const isWorldTreeActive = ref(false);

// Computed properties
const worldTreeIncomes = computed(() => {
  return isWorldTreeActive.value ? [worldTree] : [];
});

const farm = computed(() => {
  return [
    ...plants.value.map((plant) => ({ ...plant, type: "plant" })),
    ...mamas.value.map((mama) => ({ ...mama, type: "mama" })),
    ...saplings.value.map((sapling) => ({ ...sapling, type: "sapling" })),
    ...worldTreeIncomes.value,
  ];
});

// Methods
const addFormItem = (type) => {
  switch (type) {
    case "mama":
      mamas.value = [...mamas.value, { ...initialMama, id: uuidGen() }];
      break;
    case "sapling":
      saplings.value = [
        ...saplings.value,
        { ...initialSapling, id: uuidGen() },
      ];
      break;
    case "plant":
      plants.value = [
        ...plants.value,
        {
          id: uuidGen(),
          hoursLeft: 0,
          minutesLeft: 0,
          productionLe: 0,
          productionHours: 0,
        },
      ];
      break;
    default:
      break;
  }
};

const removeFarmItem = (itemToRemove, type) => {
  switch (type) {
    case "mama":
      mamas.value = mamas.value.filter((item) => itemToRemove.id !== item.id);
      break;
    case "sapling":
      saplings.value = saplings.value.filter(
        (item) => itemToRemove.id !== item.id
      );
      break;
    case "plant":
      plants.value = plants.value.filter((item) => itemToRemove.id !== item.id);
      break;
    default:
      break;
  }
};

const updateFarmItem = (newFarmItemObject, type) => {
  switch (type) {
    case "mama":
      const mamaIndex = mamas.value.findIndex(
        (mama) => mama.id === newFarmItemObject.id
      );
      mamas.value[mamaIndex] = newFarmItemObject;
      break;
    case "sapling":
      const saplingIndex = saplings.value.findIndex(
        (sapling) => sapling.id === newFarmItemObject.id
      );
      saplings.value[saplingIndex] = newFarmItemObject;
      break;
    case "plant":
      const plantIndex = plants.value.findIndex(
        (plant) => plant.id === newFarmItemObject.id
      );
      plants.value[plantIndex] = newFarmItemObject;
      break;
    default:
      break;
  }
};
</script>

<template>
  <FarmItemGenerator @add-farm-item="addFormItem" />
  <div class="plants">
    <Plant
      v-for="plant in plants"
      :key="`plant-${plant.id}`"
      :plant="plant"
      @remove-plant="removeFarmItem"
      @update-plant="updateFarmItem"
    />
  </div>
  <div class="saplins">
    <Sapling
      v-for="sapling in saplings"
      :key="`sapling-${sapling.id}`"
      :sapling="sapling"
      @remove-sapling="removeFarmItem"
      @update-sapling="updateFarmItem"
    />
  </div>
  <div class="mamas">
    <Mama
      v-for="mama in mamas"
      :key="`mama-${mama.id}`"
      :mama="mama"
      @remove-mama="removeFarmItem"
      @update-mama="updateFarmItem"
    />
  </div>

  <CalendarTable
    :farm="farm"
    v-model:is-world-tree-active="isWorldTreeActive"
  ></CalendarTable>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.plants,
.saplins,
.mamas {
  display: flex;
  justify-content: space-between;
  align-items: center;
  overflow: auto;
}

.input-group {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column nowrap;
  margin: 0.5rem auto;
}

.farm-item__container {
  padding: 2rem;
  margin: 3rem;
  border: 1px solid black;
  border-radius: 7px;
  max-width: fit-content;
}

.farm-item__container button {
  margin: 1rem 0 0;
}

.farm-item__container > h4 {
  margin: 0 0 1rem;
}

.btn {
  border: none;
  border-radius: 7px;
  padding: 0.3rem 0.7rem;
  font-weight: bold;
}

.btn--remove-item {
  color: white;
  background: tomato;
}

.btn--add-item {
  color: white;
  background: #159515;
}

.farm-item__images > img {
  display: block;
  cursor: default;
}

.farm-item__images img:nth-child(even) {
  position: relative;
  transform: translateY(-50px);
  margin: 0 auto -50px;
  z-index: -1;
}
</style>
