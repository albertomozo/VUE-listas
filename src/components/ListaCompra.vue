<script setup>
    import {reactive} from 'vue';
    import InputView from './InputView.vue';

    defineProps({
    msg: {
        type: String,
        required: true
    }
    })

    
    let  lista = reactive([]);
  
  
  
    function   handleNewItem(texto) {
        console.log('entro enter');
        console.log(texto.value);
;                if (texto.value !== "" ) {
                lista.push(texto.value);
                texto.value = "";
                console.log(lista);
                return;
            }
            if (texto.length >= MAX)
            {
                console.log('te pasaste');
                alert('ya no puedes meter más caracteres');
                return;
            }
    }
    function deleteTag(text) {
       /*  lista = lista.filter((item) => item !== text); */
        const index = lista.indexOf(text);
        if (index !== -1) {
            lista.splice(index, 1);
        }
       
      
    }     
    
    
            
         
  

</script>

<template>
    <div class="lista">
        <h1>{{ msg  }}</h1>
        <InputView @on-new-item="(text) => handleNewItem(text)" />
          
      
        <div class="tags">
            <div class="tag" v-for="(producto, index) in lista" :key="index">
            {{ producto }} <button @click="() => deleteTag(producto)">X</button>

            </div>
        </div>
    </div>

</template>

<style scoped>
.lista {
    margin :5px;
    border : 1px solid black;
    padding: 2px 5px;
    background-color: aquamarine;

}

.tag {
    margin : 1px;
    border-radius: 3px;
    border : 1px solid blue;
    background : whitesmoke;
}


</style>