<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <header>

    <div class="wrapper">
      <HelloWorld msg="Prueba tecnica" />
      <nav class="nav">
        <RouterLink class="nav-link" to="/">Algoritmo Cesar</RouterLink>
        <RouterLink class="nav-link" to="/libros">Libros</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 1rem;
  text-align: center;
  margin-top: 0;
  margin-bottom: 2em;
}
nav a {
  color: hsla(160, 100%, 37%, 1);
}
nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  /* background-color: transparent; */
}
.nav-link:hover{
  color: var(--color-text);
}
nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
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

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;
    padding: 1rem 0;
    margin-top: 0;
    margin-bottom: 1.1em;
  }
}
@media (max-width: 1023px) {
  nav {
    justify-content: center;
  }
}
</style>
