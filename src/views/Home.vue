<template>
  <div class="page">
    <input
      type="text"
      placeholder="Enter pokemon here"
      class="input-pokemon"
      v-model.trim="text"
    />
  </div>
  <div class="container">
    <div
      class="container-pokemons"
      v-for="(pokemon, idx) in filteredPokemon"
      :key="idx"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </router-link>
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
      text: "",
      filteredPokemon: computed(() => updatePokemon()),
    });

    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0&limit=151")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce((acc, cur, idx) => 
        acc = { ...acc, [cur.name]: idx + 1 }
        ,{})
      })
      return { ...toRefs(state) };
  },
};
</script>

<style scoped>
.page {
  width: 100%;
  display: flex;
  justify-content: center;
}
.input-pokemon {
  padding: 8px;
  border: solid 2px;
  color: rgb(28, 28, 192);
}
.container {
  margin-top: 20px;
  padding: 16px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.container-pokemons {
  margin-left: 16px;
  font-size: 12px;
  color: rgb(27, 27, 228);
}
</style>
