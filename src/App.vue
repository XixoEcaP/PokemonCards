<template>
  <div id="app">
    <PokemonCard />
  </div>
</template>

<script>
import PokemonCard from './components/PokemonCard.vue';

export default {
  components: {
    PokemonCard
  }
};
</script>

<style>
/* Add global styles here */
body {
  padding: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url("/resources/pokemon2.png");
  background-size: cover;
}
</style>
