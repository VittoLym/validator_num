<script setup>
import * as XLSX from 'xlsx'
import { ref } from 'vue';
import ChooseSheet from './components/ChooseSheet.vue';

const excelData = ref([]);
const selectedSheet = ref([])
const sheetNames = ref([])
let workbook = null; 

const handleFileUpload = (e) => {

  const file = e.target.files[0]
  if(!file) return;

  const reader = new FileReader();

  reader.onload = (e) => {
    const data = new Uint8Array(e.target.result);
    workbook = XLSX.read(data, { type: 'array'});
    
    sheetNames.value = workbook.SheetNames
    selectedSheet.value = workbook.SheetNames[0];
    loadSheetData();
  }
  reader.readAsArrayBuffer(file); 
}

const loadSheetData = () => {
  const sheet = workbook.Sheets[selectedSheet.value];
  const sheetData = XLSX.utils.sheet_to_json(sheet, { header: 1 });
  
  // Cargar solo las primeras 50 filas para evitar que la computadora se bloquee
  excelData.value = sheetData.slice(0, 50);
};

</script>

<template>
  <div>
    <h1>Put Archive Please</h1>
    <ChooseSheet :sheetNames="sheetNames" v-if="sheetNames.length > 0"/>
    <input type="file" @change="handleFileUpload" v-else>
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
