<script setup>
import { ref, watch } from "vue";

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
  <div class="Plant farm-item__container">
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
    <button type="button" class="btn btn--remove-item" @click="removePlant">
      Quitar
    </button>
  </div>
</template>
