<script setup>

 

 
</script>

<template>
  <header>
   
  
  <nav>
      <ul>
        <li>
            <RouterLink to="/"  active-class="active">Home</RouterLink>
        </li>
        <li>
            <RouterLink to="/tableros"  active-class="active">listas</RouterLink>
       </li>
    </ul>
 </nav>
 </header>
 <RouterView />
<!--   <header>
    
    <div class="wrapper">
      

    </div>
  </header> -->

 
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

  nav ul {  
    list-style: none;
    padding: 0;
    margin: 0;
  }

  nav ul li {
    display: inline-block;
    padding: 0.5rem 1rem;
  }

  nav ul li a {
    text-decoration: none;
    color: inherit;
  }

  nav ul li a:hover {
    text-decoration: underline;
  }

  .active {
      background-color: aqua;
  }


</style>
