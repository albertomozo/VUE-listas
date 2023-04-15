<script setup>
  import { createApp,reactive } from 'vue';
  import ListaCompra from './components/ListaCompra.vue';
 const  tableros = reactive([{ id:'1', textoLista: 'Lista 1'}]);
 

  function nuevaLista()
  {
      const textoLista = prompt('Ingrese el texto de la lista');
  /*    const app = createApp(ListaCompra, { msg: textoLista });
      const nuevaInstancia = app.mount(document.createElement('div'));
      document.querySelector(".wrapper").appendChild(nuevaInstancia.$el); */
      
   
    if (textoLista) {
      const tablero = {
        id: crypto.randomUUID(),
        textoLista : textoLista
      } 
      tableros.push(tablero);
    }
  
  }
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="50" height="50" />
    <button @click="nuevaLista">Nueva lista</button>
    <div class="wrapper">
     <!--  <ListaCompra msg="Primera Lista" />
      <ListaCompra msg="Segunda Lista" /> -->
      <ListaCompra v-for="tablero in tableros" 
        :msg="tablero.textoLista" 
        :tableroId="tablero.id"
        :key="tablero.id" 
      />
    </div>
  </header>

 
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}


  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

</style>
