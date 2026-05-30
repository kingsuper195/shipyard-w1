<script setup>
import { ref, watch, onMounted } from 'vue';

const pntData = ref([
  { "hours": 0, "multi": 1 },
  { "hours": 0, "multi": 1 },
  { "hours": 0, "multi": 1 },
  { "hours": 0, "multi": 1 },
  { "hours": 0, "multi": 1 },
  { "hours": 0, "multi": 1 },
  { "hours": 0, "multi": 1 },
  { "hours": 0, "multi": 1 },
]);

onMounted(() => {
  const savedContent = localStorage.getItem("pntData");
  if (savedContent) {
    pntData.value = JSON.parse(savedContent);
  }
});

const saveToLocalStorage = () => {
  try {
    localStorage.setItem("pntData", JSON.stringify(pntData.value));
    console.log("Set.")
  } catch (error) {
    console.error("Failed to save to localStorage:", error);
  }
};
</script>

<template>
  <table>
    <tbody>
      <tr>
        <th>Week</th>
        <th>Hours</th>
        <th>Multiplier</th>
        <th>Points</th>
      </tr>
      <tr v-for="n in 8">
        <td>{{ n - 1 }}</td>
        <td class="w1 hours"><input v-model="pntData[(n - 1)].hours"></td>
        <td class="w1 multi"><input v-model="pntData[(n - 1)].multi"></td>
        <td class="w1 pnts">{{ Math.round((n - 1) == 0 ? ((pntData[(n - 1)].hours * 5) * pntData[(n - 1)].multi + 10) /
          2 :
          (pntData[(n - 1)].hours * 5) * pntData[(n - 1)].multi + 35) }}</td>
      </tr>
      <tr>
        <td>Total</td>
        <td>{{pntData.map((e) => parseInt(e.hours)).reduce((sum, num) => sum + num, 0)}}</td>
        <td></td>
        <td>{{Math.round(pntData.map((e, i) => i == 0 ? ((e.hours * 5) * e.multi + 10) / 2 : (e.hours * 5) * e.multi +
          35).reduce((sum, num) => sum + num, 0))}}</td>
      </tr>
    </tbody>
  </table>
  <button v-on:click="saveToLocalStorage">Save</button>
</template>

<style scoped></style>
