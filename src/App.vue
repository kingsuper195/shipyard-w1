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
  "Stickers": { "points": 20, "img": "shop/pile_of_stickers.png" },
  "Hot Choco": { "points": 30, "img": "shop/hotchoco.webp" },
  "Domain Grant": { "points": 40, "img": "shop/domain.png" },
  "Chrome License": { "points": 50, "img": "shop/chrome.webp" },
  "$10 Tech Grant": { "points": 60, "img": "shop/tech.png" },
  "AI Credits": { "points": 100, "img": "shop/ai.webp" },
  "Pico 9": { "points": 100, "img": "shop/pico9.webp" },
  "Nebula": { "points": 120, "img": "shop/nebula.webp" },
  "Battleship Boardgame": { "points": 180, "img": "shop/battleships.jpeg" },
  "Subnautica Below Zero": { "points": 200, "img": "shop/subnautica.jpg" },
  "Small Blåhaj": { "points": 220, "img": "shop/smallblahaj.webp" },
  "Raspberry Pi Zero 2 W": { "points": 260, "img": "shop/rpio2w.png" },
  "Pinecil": { "points": 280, "img": "shop/pinecil.webp" },
  "Big Blåhaj": { "points": 320, "img": "shop/bigblahaj.webp" },
  "YubiKey": { "points": 400, "img": "shop/yubikey.webp" },
  "Nothing Headphones": { "points": 450, "img": "shop/nothingheaphones.webp" },
  "A1 Mini": { "points": 550, "img": "shop/a1mini.webp" },
  "Flipper Zero": { "points": 600, "img": "shop/flipperzero.webp" },
  "iPad Air": { "points": 700, "img": "shop/ipadair.webp" },
  "Ugee Tablet": { "points": 700, "img": "shop/ugeedrawingtablet.webp" },
  "A1": { "points": 850, "img": "shop/a1.webp" },
  "MacBook Neo": { "points": 900, "img": "shop/macbookneo.webp" },
  "Framework 12": { "points": 1000, "img": "shop/framework12.webp" },
  "Framework 13": { "points": 1100, "img": "shop/framework13.webp" },
  "Framework 16": { "points": 1200, "img": "shop/framework16.webp" }
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
  return Math.round(pntData.value.map((e, i) => i == 0 ? ((e.hours * 5) * e.multi + 10) / 2 : (e.hours * 5) * e.multi + 35).reduce((sum, num) => sum + num, 0))
}
</script>

<template>
  <div id="main">
    <h1>Shipyard time Est</h1>
    <div id="flex">
      <table class="section">
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
            <td class="w1 pnts">{{ Math.round((n - 1) == 0 ? ((pntData[(n - 1)].hours * 5) * pntData[(n - 1)].multi +
              10)
              /
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
      <button  v-on:click="saveToLocalStorage">Save</button>
      <div class="section">
        <ul>
          <li v-for="prize in Object.keys(prizes)"><img v-bind:src="prizes[prize].img">{{ prize }} - {{
            prizes[prize].points < total() ? `YES! You could get ${Math.floor(total() / prizes[prize].points)}.` : `No.
              You would need ${prizes[prize].points - total()} more.` }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Gaegu&family=Permanent+Marker&display=swap');

#main {
  font-family: "Gaegu", sans-serif;
  font-weight: 400;
  font-style: normal;
  padding: 1em;
  background: linear-gradient(180deg, #3BB1FF 0%, #339AD0 15%, #3070BA 41.83%, #2A68B0 52.41%, #235C9F 71.64%, #1F5390 87.98%);
  color: white;
  width: 100%;
  height: 100%;
  overflow: hidden;
  text-align: center;

}

#flex {
  display: flex;
  height: 85%;
}

h1 {
  font-family: "Permanent Marker", cursive;
  font-weight: 400;
  font-style: normal;
  padding: 0.1em;
  margin: 0.1em;
}

.section {
  padding: 1em;
  padding: 1em;
  background: #346daf99;
  color: white;
  border: 1px solid black;
  border-radius: 3%;
  margin: 0.5em;
  overflow: auto;
  width: 44%;
  scrollbar-width: none;
}

button {
  padding: 1em;
  background: #346daf99;
  color: white;
  border: 1px solid black;
  border-radius: 3%;
  margin: 0.5em;
  overflow: auto;
  width:5%;
}

button:hover {
  background: #144d9f99;
}

img {
  max-height: 8rem;
  display: block;
  margin: auto;
}

ul {
  list-style-type: none;
}

li {
  padding: 1em;
  background: #144d9f99;
  color: white;
  border: 1px solid black;
  border-radius: 3%;
  margin: 0.5em;
  overflow: auto;
}

th,
td {
  border: 1px solid;
  padding: 0.1em;
}
</style>
