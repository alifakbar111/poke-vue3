<template>
  <div
    v-if="pokemon"
    className="w-3/12 m-auto bg-purple-50 mt-4 p-4 shadow-2xl flex justify-center flex-col items-center"
  >
    <h3 className="text-2xl text-green-900 uppercase">{{ pokemon.name }}</h3>
    <div class="flex justify-center">
      <img className="w-48" :src="pokemon.sprites.front_shiny" alt="" />
      <img className="w-48" :src="pokemon.sprites.back_shiny" alt="" />
    </div>
    <h3 class="text-yellow-400">Types</h3>
    <div v-for="(type, idx) in pokemon.types" :key="idx">
      <h5 class="text-blue-900">{{ type.type.name }}</h5>
    </div>
  </div>
</template>
<script>
import { useRoute } from "vue-router";
import { reactive, toRefs } from "vue";
export default {
  setup() {
    const route = useRoute();
    const pokemon = reactive({ pokemon: null });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then(res => res.json())
      .then(data => {
        pokemon.pokemon = data;
        console.log("pokemon data", data);
      });
    return { ...toRefs(pokemon) };
  }
};
</script>
<style lang=""></style>
