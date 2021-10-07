<script setup>
import { ref, computed } from "vue";

defineProps({
  daysWithProduction: Array,
});

const dayLimit = ref(new Date());

const dates = computed(() => {
  const dates = [];

  const today = new Date();
});
</script>

<template>
  <div class="CalendarTable">
    <input type="date" v-model="dayLimit" />
    <table>
      <thead>
        <tr>
          <td></td>
        </tr>
      </thead>
    </table>
  </div>
</template>

<style scoped>
.CalendarTable {
  margin: 3rem;
  padding: 1rem 0;
  border: 1px solid black;
}
</style>
