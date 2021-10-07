<script setup>
import { ref } from "vue";

defineProps({
  mama: Object,
});

const hoursLeft = ref(0);
const minutesLeft = ref(0);
</script>

<template>
  <span>Mama</span>
  <input type="number" v-model="hoursLeft" />
  <input type="number" v-model="minutesLeft" />
</template>

<style scoped></style>
