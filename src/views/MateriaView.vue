<script setup>
import axios from "axios";
import { ref } from "vue";
import { useRoute, useRouter } from "vue-router";


const router = useRouter();
const route = useRoute();



const back = () => {
    router.push("/materias");
};
let materias = ref('');

const getData = async () => {
    try {
        const  {data} = await axios.get("/public/materias.json");
        materias.value = data.cursos;
        console.log('materias.value ')
        console.log(materias.value);
        const indice = materias.value.findIndex(p => p.materia === route.params.name);
       console.log('indice : '+ indice)
        const matSel = materias[indice];
    } catch (error) {
        console.log(error);
        materias.value = null;
    }
};

getData();
</script>

<template>
    <h1>Materia : {{ $route.params.name }}</h1>
        <p>Nombre : {{ matSel }}</p>
        <!-- <img v-bind:src="{{matSel.imagen}}" alt="" /> -->
     
</template>
