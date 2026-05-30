<script setup>
import { ref, onMounted } from 'vue';

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

const prizes = ref({
  "Stickers": 20,
  "Hot Choco": 30,
  "Domain Grant": 40,
  "Chrome License": 50,
  "$10 Tech Grant": 60,
  "AI Credits": 100,
  "Pico 9": 100,
  "Nebula": 120,
  "Battleship Boardgame": 180,
  "Subnautica Below Zero": 200,
  "Small Blåhaj": 220,
  "Raspberry Pi Zero 2 W": 260,
  "Pinecil": 280,
  "Big Blåhaj": 320,
  "YubiKey": 400,
  "Nothing Headphones": 450,
  "A1 Mini": 550,
  "Flipper Zero": 600,
  "iPad Air": 700,
  "Ugee Tablet": 700,
  "A1": 850,
  "MacBook Neo": 900,
  "Framework 12": 1000,
  "Framework 13": 1100,
  "Framework 16": 1200
});

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

const total = () => {
  return Math.round(pntData.value.map((e, i) => i == 0 ? ((e.hours * 5) * e.multi + 10) / 2 : (e.hours * 5) * e.multi +35).reduce((sum, num) => sum + num, 0))
}
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
        <td>{{ total() }}</td>
      </tr>
    </tbody>
  </table>
  <button v-on:click="saveToLocalStorage">Save</button>
  <ul>
    <li v-for="prize in Object.keys(prizes)">{{ prize }} - {{prizes[prize] < total() ? `YES! You could get ${Math.floor(total()/prizes[prize])}.` : `No. You would need ${prizes[prize] -total()} more.` }}</li>
  </ul>
</template>

<style scoped></style>
