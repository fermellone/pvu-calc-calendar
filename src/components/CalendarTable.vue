<script setup>
import { ref, computed } from "vue";
import {
  eachDayOfInterval,
  addDays,
  addMonths,
  addHours,
  isAfter,
  add,
  format,
} from "date-fns";

// Props
const props = defineProps({
  farm: Array,
  isWorldTreeActive: Boolean,
});

// Constants
const today = format(new Date(), "yyyy-MM-dd");
const initialEndDate = format(addDays(new Date(), 6), "yyyy-MM-dd");
const tomorrow = format(addDays(new Date(), 1), "yyyy-MM-dd");
const maxDate = format(addMonths(new Date(), 2), "yyyy-MM-dd");

// Data
const startDate = ref(today);
const endDate = ref(initialEndDate);
const leToPvuPrice = ref(605);
const currentLe = ref(0);
const currentPvu = ref(0);
const currentSaplings = ref(0);

// Emits
const emit = defineEmits(["update:is-world-tree-active"]);

// Computed properties
const interval = computed(() => {
  const datesOfInterval = eachDayOfInterval({
    start: addDays(new Date(startDate.value), 1),
    end: addDays(new Date(endDate.value), 1),
  });

  return datesOfInterval;
});

const worldTreeCheckbox = computed({
  get() {
    return props.isWorldTreeActive;
  },
  set(newValue) {
    emit("update:is-world-tree-active", newValue);
  },
});

const totalIncomes = computed(() => {
  return interval.value.reduce((total, currentDate) => {
    const daysWithIncomes = props.farm.filter((item) =>
      checkIfTableDateMatchItemHarvestDate(item, currentDate)
    );
    let subTotal = calcIncomesByDay(daysWithIncomes);
    return total + subTotal;
  }, 0);
});

const calculatedPvuWithCurrentLePrice = computed(() => {
  return Number(
    ((totalIncomes.value + calculatedTotalLe.value) / leToPvuPrice.value) * 0.95
  );
});

const calculatedTotalPvu = computed(() => {
  return Number(calculatedPvuWithCurrentLePrice.value + currentPvu.value);
});

const calculatedTotalLe = computed(() => {
  return Number(totalIncomes.value + currentLe.value);
});

const worldTreeSaplings = computed(() => {
  return Number(4 * interval.value.length);
});

const totalPlantsThatCanBeBought = computed(() => {
  const availableSapplinsWithLe = Math.floor(calculatedTotalLe.value / 100);
  const worldTreeSaps = worldTreeCheckbox.value ? worldTreeSaplings.value : 0;

  const necesaryPvuForGerm = Math.floor(
    ((availableSapplinsWithLe + currentSaplings.value + worldTreeSaps) / 100) *
      4
  );

  const availableLe =
    calculatedTotalLe.value -
    (necesaryPvuForGerm - currentPvu.value) * leToPvuPrice.value;

  const saplingsToBuyWithLe = Math.floor(availableLe / 100);

  return Number(
    (saplingsToBuyWithLe + currentSaplings.value + worldTreeSaps) / 100
  );
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
    isAfter(new Date(endDate.value), harvestDate) &&
    !!farmItem.productionHours
  ) {
    harvestDate = addHours(new Date(harvestDate), farmItem.productionHours);

    harvestDates.push(formatDate(harvestDate));
  }

  return harvestDates.includes(formatDate(date));
};

const calcIncomesByDay = (daysWithIncomes) => {
  return daysWithIncomes.reduce((subTotal, currentDay) => {
    return subTotal + currentDay.productionLe;
  }, 0);
};
</script>

<template>
  <div class="CalendarTable">
    <div class="input-group">
      <label>Fecha inicial</label>
      <input type="date" v-model="startDate" :min="today" :max="maxDate" />
    </div>
    <div class="input-group">
      <label>Fecha final</label>
      <input type="date" v-model="endDate" :min="tomorrow" :max="maxDate" />
    </div>
    <div class="input-group">
      <label for="world-tree-checkbox">Arbol del mundo</label>
      <input
        type="checkbox"
        v-model="worldTreeCheckbox"
        id="world-tree-checkbox"
      />
    </div>
    <div class="input-group">
      <label>Precio actual LE -> PVU</label>
      <input type="number" v-model="leToPvuPrice" />
    </div>
    <div class="input-group">
      <label>LE actuales</label>
      <input type="number" v-model="currentLe" />
    </div>
    <div class="input-group">
      <label>PVU actuales</label>
      <input type="number" v-model="currentPvu" />
    </div>
    <div class="input-group">
      <label>Saplings actuales</label>
      <input type="number" v-model="currentSaplings" />
    </div>
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
                ? `${item.productionLe} LE`
                : ""
            }}
          </td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td :colspan="interval.length">Total</td>
          <td>{{ totalIncomes }} LE</td>
        </tr>
      </tfoot>
    </table>
    <div class="saplins-section">
      <h4>Recompensas</h4>
      <p v-if="isWorldTreeActive">
        Saplings del árbol del mundo (4 por día):
        {{ worldTreeSaplings }} saplings.
      </p>
      <p>
        PVUs al cambio actual:
        {{ calculatedPvuWithCurrentLePrice.toFixed(2) }} PVU.
      </p>
      <h4>Al final del ciclo</h4>
      <p>
        LEs totales:
        {{ calculatedTotalLe.toFixed(2) }} LE.
      </p>
      <p>
        PVUs totales:
        {{ calculatedTotalPvu.toFixed(2) }} PVU.
      </p>
      <h4>Cambiando saplings por semillas</h4>
      <p>Plantas: {{ totalPlantsThatCanBeBought.toFixed(4) }}</p>
    </div>
  </div>
</template>

<style scoped>
.CalendarTable {
  margin: 3rem auto;
  padding: 1rem 0;
  border: 1px solid black;
  border-radius: 7px;
  text-align: center;
  overflow: auto;
  min-width: 90vw;
}

.CalendarTable > .input-group {
  display: inline-flex;
  margin: 0 1rem;
}

@media screen and (max-width: 450px) {
  .CalendarTable > .input-group {
    display: flex;
    margin: 0.5rem auto;
  }
}

table {
  border-collapse: collapse;
  margin: 2rem auto;
  font-size: 0.9em;
  font-family: sans-serif;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

thead tr {
  background-color: #159515;
  color: #ffffff;
  text-align: left;
}

thead tr th {
  white-space: nowrap;
}

thead tr th,
thead tr td {
  padding: 0.9rem 1rem;
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

tfoot {
  font-weight: bold;
}

tfoot tr td:first-of-type {
  text-align: left;
}
</style>
