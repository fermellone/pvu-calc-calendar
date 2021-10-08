<script setup>
import { ref } from "vue";

const type = ref("sapling");

const emit = defineEmits(["add-farm-item"]);

const addFarmItem = () => {
  emit("add-farm-item", type.value);
};
</script>

<template>
  <div class="FarmItemGenerator">
    <div class="input-group">
      <label>Tipo de planta</label>
      <select v-model="type">
        <option value="sapling" key="sapling">Retoño</option>
        <option value="mama" key="mama">Mama</option>
        <option value="plant" key="plant">NFT</option>
      </select>
    </div>
    <button role="button" class="btn btn--add-item" @click="addFarmItem">
      Agregar
    </button>
  </div>
</template>

<style scoped>
.FarmItemGenerator {
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  justify-content: center;
  margin: 3rem;
}
</style>
