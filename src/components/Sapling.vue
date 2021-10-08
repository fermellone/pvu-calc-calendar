<script setup>
import { ref, watch } from "vue";
import saplingSvg from "../assets/sapling.svg";
import landSvg from "../assets/land.svg";

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
  <div class="farm-item__container">
    <h4>Sapling</h4>
    <div class="farm-item__images">
      <img :src="saplingSvg" alt="sunflower mama" />
      <img :src="landSvg" alt="land" />
    </div>
    <div class="input-group">
      <label>Horas Restantes</label>
      <input type="number" v-model="hoursLeft" />
    </div>
    <div class="input-group">
      <label>Minutos Restantes</label>
      <input type="number" v-model="minutesLeft" />
    </div>
    <button type="button" class="btn btn--remove-item" @click="removeSapling">
      Quitar
    </button>
  </div>
</template>
