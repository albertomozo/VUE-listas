<script setup>
    import {reactive} from 'vue';
    import InputView from './InputView.vue';
    import BotonesTablero from './BotonesTablero.vue';
    /* import the fontawesome core */
    import { library } from '@fortawesome/fontawesome-svg-core'
    /* import font awesome icon component */
    import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
    /* import specific icons */
    import { faPenToSquare } from '@fortawesome/free-solid-svg-icons'
    import { faTrashCan } from '@fortawesome/free-solid-svg-icons'
    import TableroForm from './TableroForm.vue';
    /* add icons to the library */
    library.add(faPenToSquare)
    library.add(faTrashCan) 


    

   
    let  tableros = reactive([
        { id:crypto.randomUUID(), 
            nombre: 'Desayuno',
            editar : false,
            items : [{ id:crypto.randomUUID(), elemento : 'pan'},
            { id:crypto.randomUUID(), elemento : 'leche'},
            { id:crypto.randomUUID(), elemento : 'cafe'}
            ]
        },
        { id:crypto.randomUUID(), 
            nombre: 'Primer Plato',
            editar : false,   
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
            if (texto.value !== "" ) {              
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
    
    function deleteTablero(tablero){
          if (confirm('¿ Estas seguro de borrar el tablero' + tablero.nombre + '?')) {  
            console.log (tablero.id)         
            const index = tableros.findIndex(t => t.id === tablero.id);
  
            // Si se encuentra el tablero, borrarlo
            if (index !== -1) {
                tableros.splice(index, 1);
            }
        
          }

    }
    
    function onDrop(evt,board,productoId){
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
        //board.items.push(productoOrigen); // Se carga al final
        console.log('productoId' + productoId)
        const indice = board.items.findIndex(p => p.id === productoId);
        console.log(indice)
        // concatenar 0-indice  + productoOrigen.id + idice +1 hasta el final
        let parte1 = board.items.filter((i,index)=> index <=  indice);
        console.log(parte1);
        parte1.push(productoOrigen);
        console.log(parte1);
        const parte2 = board.items.filter((i,index)=> index >  indice);
        console.log(parte2);
        board.items = parte1.concat(parte2);
      


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
<div class="contenedor_tableros">
    <button @click="nuevaLista">Nueva lista</button>
    <button>Guardar Tableros</button>
    <BotonesTablero msg="Guardar Tableros"  @click="guardarTablero"/>
   <div class="tableros">
    <div class="lista" v-for="tablero in tableros"  v-bind:style = "{backgroundColor : tablero.color }">
        <div class="tablero">        
            <h1>{{ tablero.nombre  }} 
                <font-awesome-icon  icon="fa-solid fa-pen-to-square"  id="formId" @click="tablero.editar = !tablero.editar" />
                <font-awesome-icon icon="fa-solid fa-trash-can"  @click="deleteTablero(tablero)"/>
            </h1>
            <TableroForm :tablero="tablero" v-show="tablero.editar" />
            
            <InputView @onNewItem="(item)=>handleNewItem(item,tablero)" />
            
            <div class="tags"
                
                @dragover.prevent
                @dragenter.prevent  
            >
                <div v-if="tablero.items.length==0" draggable="true"  >No hay elementos</div>
                <div class="tag" v-for="producto in tablero.items" 
                    draggable="true" 
                    @dragstart="startDrag($event, tablero.id, producto.id)"
                    @drop="onDrop($event,tablero,producto.id)"
                >
                {{ producto.elemento }} <button @click="() => deleteTag(producto.id,tablero)">X</button>

                </div>
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
.tableros {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: start;
    align-items: auto;
    align-content: start;
    margin :5px;
    border : 1px solid black;
    padding: 2px 5px;
    background-color: rgb(105, 108, 107)38, 232);
}
.tablero {
  flex: 0 0 auto;
  margin: 10px;
}

.contenedor_tableros{
    width: auto}
  





</style>