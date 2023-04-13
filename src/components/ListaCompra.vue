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
                if (texto.length>=MAX)
                {
                    console.log('te pasaste');
                    alert('ya no puedes meter más caracteres');
                    return;
                }
            }      
       
       
            
         
  

</script>

<template>
    <div>
        <h1>{{ msg  }}</h1>
        <InputView @on-new-item="(text) => handleNewItem(text)" />
          
      
        <div class="tags">
            <div class="tag" v-for="(producto, index) in lista" :key="index">
            {{ producto }} 
            </div>
        </div>
    </div>

</template>