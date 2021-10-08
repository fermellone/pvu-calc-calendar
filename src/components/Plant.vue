<script setup>
import { ref, defineEmits, watch } from "vue";

// Props
const props = defineProps({
  plant: Object,
});

// Data
const hoursLeft = ref(props.plant.hoursLeft);
const minutesLeft = ref(props.plant.minutesLeft);
const productionLe = ref(props.plant.productionLe);
const productionHours = ref(props.plant.productionHours);

// Emits
const emit = defineEmits(["remove-plant"]);

const removePlant = () => {
  emit("remove-plant", props.plant, "plant");
};

const updatePlant = (newPlantObject) => {
  // if (currentValue < 0 || currentValue > 60 || isNaN(currentValue)) return;
  emit("update-plant", newPlantObject, "plant");
};

// Watchers
watch(hoursLeft, (currentValue) => {
  const newPlantObject = { ...props.plant, hoursLeft: currentValue };
  emit("update-plant", newPlantObject, "plant");
});

watch(minutesLeft, (currentValue) => {
  const newPlantObject = { ...props.plant, minutesLeft: currentValue };
  emit("update-plant", newPlantObject, "plant");
});

watch(productionLe, (currentValue) => {
  const newPlantObject = { ...props.plant, productionLe: currentValue };
  emit("update-plant", newPlantObject, "plant");
});

watch(productionHours, (currentValue) => {
  const newPlantObject = { ...props.plant, productionHours: currentValue };
  emit("update-plant", newPlantObject, "plant");
});
</script>

<template>
  <div class="Plant farm-item-container">
    <h4>Plant</h4>
    <div class="input-group">
      <label>Horas Restantes</label>
      <input type="number" v-model="hoursLeft" />
    </div>
    <div class="input-group">
      <label>Minutos Restantes</label>
      <input type="number" v-model="minutesLeft" />
    </div>
    <div class="input-group">
      <label>Producción de LE</label>
      <input type="number" v-model="productionLe" />
    </div>
    <div class="input-group">
      <label>Tiempo de producción</label>
      <input type="number" v-model="productionHours" />
    </div>
    <button type="button" @click="removePlant">X</button>
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
