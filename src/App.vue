<script setup>
import { ref } from "vue";
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import Plant from "./components/Plant.vue";
import Saplin from "./components/Saplin.vue";
import Mama from "./components/Mama.vue";
import CalendarTable from "./components/CalendarTable.vue";

const plants = ref([
  { hoursLeft: 10, minutesLeft: 25, productionLe: 2100, productionHours: 250 },
]);
const saplins = ref([]);
const mamas = ref([]);
</script>

<template>
  <div class="plants">
    <Plant
      v-for="(plant, index) in plants"
      :key="`plant-${index}`"
      :plant="plant"
    />
  </div>
  <div class="saplins">
    <Saplin
      v-for="(saplin, index) in saplins"
      :key="`saplin-${index}`"
      :saplin="saplin"
    />
  </div>
  <div class="mamas">
    <Mama v-for="(mama, index) in mamas" :key="`mama-${index}`" :mama="mama" />
  </div>

  <CalendarTable></CalendarTable>
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
</style>
