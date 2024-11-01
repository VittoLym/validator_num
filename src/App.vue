<script setup>
import * as XLSX from 'xlsx'
import { ref } from 'vue';

const excelData = ref([]);

const handleFileUpload = (e) => {
  const file = e.target.files[0]
  if(!file) return;

  const reader = new FileReader();
  reader.onload = (e) => {
    const data = new Uint8Array(e.target.result);
    const workbook = XLSX.read(data, { type: 'array'});

    const firstSheet = workbook.Sheets[workbook.SheetNames[0]]
    excelData.value = XLSX.utils.sheet_to_json(firstSheet, {header: 1})
  }
  reader.readAsArrayBuffer(file);
}


</script>

<template>
  <div>
    <h1>Put Archive please</h1>
    <input type="file" @change="handleFileUpload">
    <div v-if="excelData.length">
      <h3>Datos de Excel:</h3>
      <table>
        <tr v-for="(row, rowIndex) in excelData" :key="rowIndex">
          <td v-for="(cell, cellIndex) in row" :key="cellIndex">{{ cell }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
