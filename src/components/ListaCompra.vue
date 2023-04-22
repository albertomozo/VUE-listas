<script setup>
    import {reactive} from 'vue';
    import InputView from './InputView.vue';

    let  tableros = reactive([
        { id:crypto.randomUUID(), nombre: 'Desayuno',
            items : [{ id:crypto.randomUUID(), elemento : 'pan'},
            { id:crypto.randomUUID(), elemento : 'leche'},
            { id:crypto.randomUUID(), elemento : 'cafe'}
            ]
        },
        { id:crypto.randomUUID(), nombre: 'Primer Plato',
            items : [{ id:crypto.randomUUID(), elemento : 'sopa'},
            { id:crypto.randomUUID(), elemento : 'ensalada'},
            { id:crypto.randomUUID(), elemento : 'arroz'}
            ]
        }

    ]);

    if (localStorage.getItem('tableros') !== null) {
           tableros = reactive(JSON.parse(localStorage.getItem('tableros')));
    }




    
    
  
  
  
    //function   handleNewItem(texto,tabId,tabNombre) {
    function   handleNewItem(texto,tablero) {
        console.log(tablero)
      
;          if (texto.value !== "" ) {
               
    
                tablero.items.push(
                  {id:crypto.randomUUID(), elemento : texto.value}  
               
                );
                texto.value = "";
                
                return;
            }
            if (texto.length >= MAX)
            {
                console.log('te pasaste');
                alert('ya no puedes meter más caracteres');
                return;
            }
    }
    function deleteTag(text,tablero) {
       /*  lista = lista.filter((item) => item !== text); */
       console.log(tablero.items);
       console.log(text);
       // const index = tablero.items.id.indexOf(text);
        const index = tablero.items.findIndex((item) => item.id === text);
        console.log(index);
        if (index !== -1) {
            console.log('borrar')
            tablero.items.splice(index, 1);
        }
       
      
    }    
    
    function onDrop(evt,board){
        console.log('ondrop')
        const { boardId, itemId } = JSON.parse(evt.dataTransfer.getData("item")); // 
        console.log({ boardId, itemId });
        const tableroOrigen  = tableros.find((tablero) => tablero.id === boardId);
        const productoOrigen  = tableroOrigen.items.find((item) => item.id === itemId); 
        console.log('nombre ' + tableroOrigen.nombre )
        console.log('producto '+ productoOrigen.elemento )      
        tableroOrigen.items = tableroOrigen.items.filter((i) => i.id !== productoOrigen.id);
        console.log(productoOrigen);
        console.log(board);
        board.items.push(productoOrigen);

    }

    function startDrag(evt,boardId,itemId) {
        console.log('start drag');
        evt.dataTransfer.dropEffect = "move";
        evt.dataTransfer.effectAllowed = "move";
        console.log(JSON.stringify({ boardId, itemId }));
        evt.dataTransfer.setData("item", JSON.stringify({ boardId, itemId }));
        
    }

    function nuevaLista()
    {
        const textoLista = prompt('Ingrese el texto de la lista');
        if (textoLista) {
            const tablero = {
                id: crypto.randomUUID(),
                nombre : textoLista,
                items : []
            } 
            tableros.push(tablero);
        }  
    }

    function guardarTablero(){
        localStorage.setItem('tableros', JSON.stringify(tableros));
    }

   
    
    
            
         
  

</script>

<template>
<div>
    <button @click="nuevaLista">Nueva lista</button>
    <button @click="guardarTablero">Guardar Tableros</button>
   
    <div class="lista" v-for="tablero in tableros" >
        <div class="tablero">        
            <h1>{{ tablero.nombre  }}</h1>
            <InputView @onNewItem="(item)=>handleNewItem(item,tablero)" />
            
            <div class="tags"
                @drop="onDrop($event,tablero)"
                @dragover.prevent
                @dragenter.prevent
            
            
            >
                <div class="tag" v-for="producto in tablero.items" 
                draggable="true"   @dragstart="startDrag($event, tablero.id, producto.id)">
                {{ producto.elemento }} <button @click="() => deleteTag(producto.id,tablero)">X</button>

                </div>
            </div>
    </div>
    </div>

    </div>

</template>

<style scoped>


.tag {
    margin : 1px;
    border-radius: 3px;
    border : 1px solid blue;
    background : whitesmoke;
}
.lista {
    
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: start;
    align-items: auto;
    align-content: start;
    margin :5px;
    border : 1px solid black;
    padding: 2px 5px;
    background-color: aquamarine;


}

.tablero {
  flex: 0 0 auto;
  margin: 10px;
}
  





</style>