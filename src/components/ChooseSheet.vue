<script setup>
import { onMounted, ref } from 'vue';
const props = defineProps(['sheetNames'])
const scrollContainer = ref(null);

onMounted(() => {
  const container = scrollContainer.value;

  container.addEventListener('wheel', (e) => {
    e.preventDefault();
    container.scrollLeft += e.deltaY; // Convierte el desplazamiento vertical en desplazamiento horizontal
  });
});
</script>
<template>
    <div v-if="props.sheetNames.length" >
        <h3>Datos de Excel:</h3>
        <main ref="scrollContainer">
            <p v-for="(item, index) in sheetNames" :key="index">{{ item }}</p>
        </main>
    </div>
</template>
<style scoped>
div{
    height: 40vh;
    width: 80vw;
    background-color: #ebddddb9;
    box-shadow: 0 0 10px #000000;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex-wrap: wrap;
    overflow: hidden;
}
h3{
    width: 100%;
    margin: 0;
    height: min-content;
    color: rgba(27, 143, 27, 0.705);
}
main{
    display: flex;
    align-items: center;
    width: 80vw;
    height: 30vh;
    overflow-x: scroll ;
}
p{
    background-color: #242424;
    box-shadow: 5px 5px 10px #0000009f;
    height: 20vh;
    flex: 1 1 auto;
    min-width: 15vw;
    max-width: 15vw;
    margin: 0 1vw;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all ease .3s;
    cursor: pointer;
}
p:hover{
    background-color: rgba(27, 143, 27, 0.705) ;
    border-radius: 8px;
    color: #000000;
}
</style>