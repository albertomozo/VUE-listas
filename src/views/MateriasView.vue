<script setup>
import axios from "axios";
import { reactive, ref } from "vue";
import { RouterLink } from "vue-router";

let materias = ref('');

const getData = async () => {
    try {
        const  {data} = await axios.get("./public/materias.json");
        console.log(data)
        materias.value = data.cursos;
        console.log(materias)
    } catch (error) {
        console.log(error);
    }
};

getData();
</script>

<template>
    <h1>Materias</h1>
    <ul>
        
        <li v-for="materia in materias">
            
            <router-link :to="`/materia/${materia.materia}`">{{
                materia.materia
            }}</router-link>
        </li>
    </ul>
</template>