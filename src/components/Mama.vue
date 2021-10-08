<script setup>
import { ref, watch } from "vue";
import mamaSvg from "../assets/mama.svg";
import landSvg from "../assets/land.svg";

// Props
const props = defineProps({
  mama: Object,
});

// Data
const hoursLeft = ref(props.mama?.hoursLeft);
const minutesLeft = ref(props.mama?.minutesLeft);

// Emits
const emit = defineEmits(["remove-mama"]);

const removeMama = () => {
  emit("remove-mama", props.mama, "mama");
};

const updateMama = (newMamaObject) => {
  emit("update-mama", newMamaObject, "mama");
};

// Watchers
watch(hoursLeft, (currentValue) => {
  const newMamaObject = { ...props.mama, hoursLeft: currentValue };
  emit("update-mama", newMamaObject, "mama");
});

watch(minutesLeft, (currentValue) => {
  const newMamaObject = { ...props.mama, minutesLeft: currentValue };
  emit("update-mama", newMamaObject, "mama");
});
</script>

<template>
  <div class="farm-item__container">
    <h4>Mama</h4>
    <div class="farm-item__images">
      <img :src="mamaSvg" alt="sunflower mama" />
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
    <button type="button" class="btn btn--remove-item" @click="removeMama">
      Quitar
    </button>
  </div>
</template>
