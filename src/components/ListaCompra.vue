<script setup>
    import {reactive} from 'vue';
    import InputView from './InputView.vue';

    defineProps({
    msg: {
        type: String,
        required: true
    },
    tableroId : {
        type :String,
        required:true
    }
    })

    
    let  lista = reactive([]);
  
  
  
    function   handleNewItem(texto) {
        console.log('entro enter');
        console.log(texto.value);
;          if (texto.value !== "" ) {
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
    
    function onDrop(evt,board){
        console.log(board); // id tablero destino 
        const { boardId, itemId } = JSON.parse(evt.dataTransfer.getData("item")); // 
        console.log({ boardId, itemId });

        console.log(lista);
        console.log(lista.boardId);
        //lista.push(

     /*   const board = tableros.find((tablero) => tablero.id === boardId);
     const item = board.items.find((item) => item.id === itemId);
        board.items = board.items.filter((i) => i.id !== item.id);
        dest.items.push({ ...item }); */

    }

    function startDrag(evt,boardId,itemId) {
        console.log('start drag');
        console.log(boardId, itemId);
        evt.dataTransfer.dropEffect = "move";
        evt.dataTransfer.effectAllowed = "move";
        console.log(JSON.stringify({ boardId, itemId }));
        evt.dataTransfer.setData("item", JSON.stringify({ boardId, itemId }));
        /* event.dataTransfer.setData("text", event.target.id); */
    }
    
    
            
         
  

</script>

<template>
    <div class="lista">
        <h1>{{ msg  }}</h1>
        <InputView @on-new-item="(text) => handleNewItem(text)" />
          
      
        <div class="tags"
            @drop="onDrop($event,tableroId)"
            @dragover.prevent
            @dragenter.prevent
        
        
        >
            <div class="tag" v-for="(producto, index) in lista" :key="index" 
            draggable="true"   @dragstart="startDrag($event, tableroId, index)">
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