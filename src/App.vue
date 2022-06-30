<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Footer from './components/Footer.vue'
</script>

<template>
  <header>
    <div class="wrapper"> 
      <div class="greetings">
        <h1 class="green">Twitch API collector</h1>
      </div> 

      <nav>
        <RouterLink to="/">Top categories</RouterLink>
        <RouterLink to="/streamers">Search streamer</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
  <Footer />
</template>

<style>
@import '@/assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  font-weight: normal;
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

a, .green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

nav { 
  font-size: 24px;
  text-align: center;
  margin-top: 0.5rem;
  margin-bottom: 20px;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

.greetings h1 {
  text-align: center;
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

@media screen and (min-width: 920px) {
  .wrapper {
    display: flex;
    flex-direction: row;
    justify-content:space-between;
    padding: 30px;
  }

  nav {
  font-size: 24px;
  text-align: center;
  margin-top: 0.5rem;
  margin-bottom: 20px;
  }
}
</style>
