<script setup>
import { ref, defineEmits, watch } from "vue";

// Props
const props = defineProps({
  sapling: Object,
});

// Data
const hoursLeft = ref(props.sapling?.hoursLeft);
const minutesLeft = ref(props.sapling?.minutesLeft);

// Emits
const emit = defineEmits(["remove-sapling", "update-hours", "update-minutes"]);

const removeSapling = () => {
  emit("remove-sapling", props.sapling, "sapling");
};

const updateSapling = (newSaplingObject) => {
  emit("update-sapling", newSaplingObject, "sapling");
};

// Watchers
watch(hoursLeft, (currentValue) => {
  const newSaplingObject = { ...props.sapling, hoursLeft: currentValue };
  updateSapling(newSaplingObject);
});

watch(minutesLeft, (currentValue) => {
  const newSaplingObject = { ...props.sapling, minutesLeft: currentValue };
  updateSapling(newSaplingObject);
});
</script>

<template>
  <div class="farm-item-container">
    <h4>Sapling</h4>
    <div class="input-group">
      <label>Horas Restantes</label>
      <input type="number" v-model="hoursLeft" />
    </div>
    <div class="input-group">
      <label>Minutos Restantes</label>
      <input type="number" v-model="minutesLeft" />
    </div>
    <button type="button" @click="removeSapling">X</button>
  </div>
</template>

<style scoped>
.farm-item-container {
  padding: 2rem;
  margin: 3rem;
  border: 1px solid black;
  max-width: fit-content;
}

.farm-item-container button {
  margin: 1rem 0 0;
}

.farm-item-container > h4 {
  margin: 0 0 1rem;
}

.input-group > * {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-flow: column nowrap;
  margin: 0 auto;
}
</style>
