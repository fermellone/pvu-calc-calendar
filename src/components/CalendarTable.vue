<script setup>
import { ref, computed } from "vue";
import {
  eachDayOfInterval,
  addDays,
  addMonths,
  addHours,
  isAfter,
  isEqual,
  add,
  format,
} from "date-fns";

// Props
const props = defineProps({
  farm: Array,
});

// Constants
const today = format(new Date(), "yyyy-MM-dd");
const initialEndDate = format(addDays(new Date(), 6), "yyyy-MM-dd");
const maxDate = format(addMonths(new Date(), 2), "yyyy-MM-dd");

// Data
const startDate = ref(today);
const endDate = ref(initialEndDate);

// Computed properties
const interval = computed(() => {
  const datesOfInterval = eachDayOfInterval({
    start: addDays(new Date(startDate.value), 1),
    end: addDays(new Date(endDate.value), 1),
  });

  return datesOfInterval;
});

// Methods
const formatDate = (date, customFormat = "dd-MMM") => {
  return format(new Date(date), customFormat);
};

const checkIfTableDateMatchItemHarvestDate = (farmItem, date) => {
  let harvestDates = [];
  let harvestDate = add(new Date(), {
    minutes: farmItem.minutesLeft,
    hours: farmItem.hoursLeft,
  });

  harvestDates.push(formatDate(harvestDate));

  while (
    isAfter(new Date(endDate.value), harvestDate && !!farmItem.productionHours)
  ) {
    harvestDate = addHours(new Date(harvestDate), farmItem.productionHours);

    harvestDates.push(formatDate(harvestDate));
  }

  return harvestDates.includes(formatDate(date));
};
</script>

<template>
  <div class="CalendarTable">
    <label>Fecha inicial</label>
    <input type="date" v-model="startDate" :min="today" :max="maxDate" />
    <label>Fecha final</label>
    <input type="date" v-model="endDate" :min="tomorrow" :max="maxDate" />
    <table>
      <thead>
        <tr>
          <th>Tipo</th>
          <th v-for="(date, index) in interval" :key="`date-${index}`">
            {{ formatDate(date) }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, rowIndex) in farm" :key="`farm-item-${rowIndex}`">
          <td>{{ item?.type }}</td>
          <td
            v-for="(date, cellIndex) in interval"
            :key="`cell-${rowIndex}-${cellIndex}`"
          >
            {{
              checkIfTableDateMatchItemHarvestDate(item, date)
                ? item.productionLe
                : ""
            }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.CalendarTable {
  margin: 3rem;
  padding: 1rem 0;
  border: 1px solid black;
  text-align: center;
  overflow: auto;
}

table {
  border-collapse: collapse;
  margin: 2rem auto;
  font-size: 0.9em;
  font-family: sans-serif;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

thead tr {
  background-color: #009879;
  color: #ffffff;
  text-align: left;
}

thead tr th,
thead tr td {
  padding: 12px 15px;
}

tbody tr {
  border-bottom: 1px solid #dddddd;
}

tbody tr:nth-of-type(even) {
  background-color: #f3f3f3;
}

tbody tr:last-of-type {
  border-bottom: 2px solid #009879;
}

tbody tr td:first-of-type {
  text-transform: capitalize;
  text-align: left;
  margin-left: 0.5em;
}
</style>
