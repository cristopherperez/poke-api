<template>
  <div class="about">
    <div v-if="pokemon" class="pokecontain">
      <div class="pokedata">
        <div class="first-info">
          <img :src="pokemon.sprites.front_default" alt="" />
          <div class="types">
            <h4>Tipo</h4>
            <div v-for="(type, idx) in pokemon.types" :key="idx">
              <h5>{{ type.type.name }}</h5>
            </div>
          </div>
        </div>
        <div class="second-info">
          <div class="basics">
            <h3>{{ pokemon.name }}</h3>
            <h5>Peso: {{ pokemon.weight }}</h5>
          </div>
          <div class="buttons">
            <button @click="showMoves()">Movimientos</button>
            <button @click="showDetails()">Descripción</button>
            <button @click="showDescription()">Información básica</button>
          </div>
        </div>
      </div>

      <div class="">
        <div>
          <p
            v-show="expandMoves"
            v-for="(move, idx) in pokemon.moves"
            :key="idx"
          >
            {{ move.move.name }}
          </p>
          <div v-if="expandDetails">
            <p>Altura: {{ pokemon.height }}</p>
            <p>Habilidades:</p>
            <div v-for="(ability, idx) in pokemon.abilities" :key="idx">
              <ul>
                <li>{{ ability.ability.name }}</li>
              </ul>
            </div>
          </div>

          <div
            v-show="expandDescription"
            v-for="(stat, idx) in pokemon.stats"
            :key="idx"
          >
            <p>
              <span>
                {{ stat.stat.name }}:</span
              >{{ stat.base_stat }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { useRoute } from "vue-router";
import { reactive, ref, toRefs } from "vue";

export default {
  setup() {
    const route = useRoute();
    const expandMoves = ref(false);
    const expandDetails = ref(false);
    const expandDescription = ref(false);
    function showMoves() {
      expandMoves.value = !expandMoves.value;
    }
    function showDetails() {
      expandDetails.value = !expandDetails.value;
    }
    function showDescription() {
      expandDescription.value = !expandDescription.value;
    }
    const state = reactive({
      pokemon: null,
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`)
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemon = data;
      });

    return {
      ...toRefs(state),
      showDetails,
      showMoves,
      showDescription,
      expandMoves,
      expandDetails,
      expandDescription,
    };
  },
};
</script>
<style scoped>
.types {
  margin-left: 20px;
  margin-bottom: 20px;
}
.first-info {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.second-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.basics {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  width: 90%;
}

button {
  margin: 0px 10px;
  background: rgb(29, 116, 247);
  color: white;
  font-family: "Press Start 2P";
  font-size: 10px;
  padding: 10px;
  border: none;
}
img {
  width: 120px;
}
h3 {
  font-size: 16px;
  color: rgb(36, 36, 36);
  text-transform: uppercase;
}
h5 {
  color: rgb(5, 5, 109);
  margin: 0;
}
p {
  font-size: 12px;
}
li {
  font-size: 10px;
}
span{
  font-size: 14px;
  margin-right: 8px;
}
.pokedata {
  display: flex;
  justify-content: center;
}
.about {
  display: flex;
  justify-content: center;
}
.pokecontain {
  background-color: rgba(138, 138, 138, 0.397);
  margin-top: 16px;
  box-shadow: 0 25px 50px -12px rgb(0 0 0 / 0.25);
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}
</style>
