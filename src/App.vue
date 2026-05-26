<script setup>
  import { ref } from 'vue'

  const pntData = ref([
    {"hours":0, "multi":1},
    {"hours":0, "multi":1},
    {"hours":0, "multi":1},
    {"hours":0, "multi":1},
    {"hours":0, "multi":1},
    {"hours":0, "multi":1},
    {"hours":0, "multi":1},
    {"hours":0, "multi":1},
  ])

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
      <tr>
        <th>1</th>
        <th class="w1 hours"><input v-model="pntData[1].hours"></th>
        <th class="w1 multi"><input v-model="pntData[1].multi"></th>
        <th class="w1 pnts">{{ (pntData[1].hours * 5) * pntData[1].multi + 35 }}</th>
      </tr>
    </tbody>
  </table>
</template>

<style scoped></style>
