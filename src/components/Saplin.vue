<script setup>
import { ref } from "vue";

defineProps({
  saplin: Object,
});

const hoursLeft = ref(0);
const minutesLeft = ref(0);
</script>

<template>
  <span>Saplin</span>
  <input type="number" v-model="hoursLeft" />
  <input type="number" v-model="minutesLeft" />
</template>

<style scoped></style>
