<template>
  <div class="w-full justify-center flex">
    <input
      type="text"
      class="border-2 p-2 border-yellow-500 rounded-md"
      placeholder="Enter pokemon name"
      v-model="pokename"
    />
  </div>
  <div class="box-content mx-auto w-1/3 mt-2">
    <div class="p-4 border-2 flex flex-wrap justify-center">
      <div
        class="ml-4 text-2xl text-white"
        v-for="(pokemon, idx) in filteredPokemon"
        :key="idx"
      >
        <h5>{{ pokemon.name }}</h5>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from "vue";
export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      pokename: "",
      filteredPokemon: computed(() => updatePokemon())
    });
    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then(res => res.json())
      .then(data => {
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      })
      .catch(err => console.error(err));

    function updatePokemon() {
      if (!state.pokename) {
        return [];
      }
      return state.pokemons.filter(pokemon =>
        pokemon.name.includes(state.pokename)
      );
    }
    return { ...toRefs(state) };
  }
};
</script>
