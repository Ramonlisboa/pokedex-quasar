<template>
   <q-modal v-model="opened" maximized no-backdrop-dismiss>
     <q-modal-layout  class="img" >
        <div class="row q-pa-md">
           <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Press+Start+2P">
          <body id="forest">
            <div id="pokedex">
              <div class="sensor">
                <!-- <button>button</button> -->
              </div>
              <div class="camera-display">
                <!-- <img src="https://assets.pokemon.com/assets/cms2/img/pokedex/full/004.png" style="width: 200px"/> -->
                <img v-if="img == 0" :src="pokemon.sprites.front_default" style="width: 15rem"/>
                <p v-if="img == 0" >Front</p>
                <img v-if="img == 1" :src="pokemon.sprites.back_default" style="width: 15rem"/>
                 <p v-if="img == 1" >Back</p>
                <img v-if="img == 2" :src="pokemon.sprites.front_shiny" style="width: 15rem"/>
                 <p v-if="img == 2" >Front Shiny</p>
                <img v-if="img == 3" :src="pokemon.sprites.back_shiny" style="width: 15rem"/>
                 <p v-if="img == 3" >Back Shiny</p>
              </div>
              <div class="divider"></div>
              <div class="stats-display" v-if="!erro">
                <p style="margin-bottom: 1rem">Name: {{pokemon.name}}</p>
                <p>Abilities:</p>
                <!-- <span>Abilities</span> -->
                <ul>
                  <li v-for="(abi, index) in pokemon.abilities" :key="index">
                    {{abi.ability.name}}
                  </li>
                  <!-- <li>Blaze</li> -->
                </ul>
                <span>Stats</span>
                <ul style="font-size:12px">
                  <li v-for="(stats, index) in pokemon.stats" :key="index">
                   {{stats.base_stat}} {{stats.stat.name}}
                  </li>
                  <!-- <li>dragon-breath</li>
                  <li>dragon-claw</li> -->
                </ul>
              </div>
              <div class="stats-display" v-else>
                <p>Nenhum pokemon encontrado</p>
              </div>
              <div class="botom-actions">
                <div id="actions">
                  <!-- <button class="a">button</button> -->
                </div>
                <div id="cross">
                  <button class="cross-button up"></button>
                  <button class="cross-button right"></button>
                  <button class="cross-button down"></button>
                  <button class="cross-button left"></button>
                  <div class="cross-button center"> </div>
                </div>
              </div>
              <div class="input-pad">
                <!-- <input /> -->
                 <q-input v-model="pesquisa"
                  inverted color="red-10" style="margin-top: 3%" float-label="Pesquise" />
              </div>
              <div class="bottom-modes">
                  <button class="level-button" @click="img=0"></button>
                  <button class="level-button" @click="img=1"></button>
                  <button class="level-button" @click="img=2"></button>
                  <button class="level-button" @click="img=3"></button>
                  <!-- <button class="pokedex-mode black-button">Pokedex</button> -->
                  <button class="game-mode black-button" @click="consultaPokemon()">Pesquisar</button>
              </div>
            </div>
          </body>
        </div>
        <!-- <div class="row absolute-bottom justify-end q-pr-md q-pb-md">
          <q-btn color="primary" @click="opened = false" size="lg" label="Close" />
        </div> -->
     </q-modal-layout>
  </q-modal>
</template>

<script>
export default {
  name: 'Pk-modal-pokedex',
  data () {
    return {
      opened: true,
      pokemon: {},
      img: null,
      pesquisa: '',
      erro: false
    }
  },
  methods: {
    consultaPokemon () {
      this.$axios.get(`https://pokeapi.co/api/v2/pokemon/${this.pesquisa}/`)
        .then((res) => {
          this.pokemon = res.data
          this.img = 0
          this.erro = false
          console.log(res.data)
        })
        .catch(() => {
          this.img = null
          this.erro = true
          this.pokemon = {}
        })
    }
  }
}
</script>

<style scoped >
/* CSS POKEDEX https://codepen.io/TabaresSotelo/pen/qYpvyq?page=1& */

.filtro-fosco {
  -webkit-filter: blur(5px);
  -moz-filter: blur(5px);
  -ms-filter: blur(5px);
  -o-filter: blur(5px);
  filter: blur(5px);
  opacity: 0.5;
}

#forest {
  width: 100%;
  height: 100%;
  background: url("https://wallpapertag.com/wallpaper/middle/6/2/2/572651-pokemon-forest-background-1920x1200-samsung.jpg");
}

#pokedex {
  width: 800px;
  height: 600px;
  margin: auto;
  background-color: #dd0b2d;
  border: #88061c 10px solid;
  border-top-width: 20px;
  z-index: 2;
  display: grid;
  grid-template-columns: 5% 40% 10% 40% 5%;
  grid-template-rows: 3% 12% 40% 20% 20% 5%;
  border-radius: 2%;
  font-family: "Press Start 2P";
}

.camera-display {
  background-color: #e5f6fc;
  border-radius: 5%;
  border: 30px solid #e6e6e6;
  grid-column-start: 2;
  grid-row-start: 3;
  grid-row-end: 5;
  img {
    width: 100%;
    height: auto;
  }
}

.divider {
  background-color: #88061c;
  grid-column-start: 3;
  grid-row-start: 1;
  grid-row-end: 7;
  margin: 0 30px;
}
.stats-display {
  background-color: #9fdab1;
  border-radius: 0%;
  border: 5px solid #e6e6e6;
  grid-column-start: 4;
  grid-row-start: 3;
  padding: 15px;
  overflow: scroll;
  font-size: 12px;
  h2 {
    margin: 10px 0;
  }
  ul {
    margin: 10px;
    overflow: auto;
  }
}

.sensor {
  button {
    cursor: pointer;
    background: #71cdf4;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    display: block;
    margin: auto;
    z-index: 3;
    float: left;
    margin-left: 25px;
    border: 3px solid white;
  }
  grid-column-start: 2;
  grid-row-start: 2;
}

.botom-actions {
  grid-column-start: 2;
  grid-row-start: 5;
  display: grid;
  grid-template-columns: 25% 25% 15% 35%;
  grid-template-rows: 10% 80% 10%;
}

#actions {
  grid-column-start: 1;
  grid-row-start: 2;
  grid-row-end: 5;
  .a {
    border-radius: 50%;
    width: 60px;
    height: 60px;
    background-color: #404040;
    border: 5px solid #303030;
    cursor: pointer;
  }
}

#cross {
  grid-column-start: 4;
  grid-row-start: 2;
  grid-row-end: 5;
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
  .cross-button {
    background-color: #404040;
    border: #303030;
    color: red;
    cursor: pointer;
    border-style: solid;
  }
  .up {
    grid-row-start: 1;
    grid-column-start: 2;
    border-width: 5px 5px 0px 5px;
  }
  .right {
    grid-row-start: 2;
    grid-column-start: 1;
    border-width: 5px 0px 5px 5px;
  }
  .down {
    grid-row-start: 3;
    grid-column-start: 2;
    border-width: 0px 5px 5px 5px;
  }
  .left {
    grid-row-start: 2;
    grid-column-start: 3;
    border-width: 5px 5px 5px 0px;
  }
  .center {
    grid-row-start: 2;
    grid-column-start: 2;
    border-width: 0;
  }
}

.input-pad {
  //background-color: green;
  grid-row-start: 4;
  grid-column-start: 4;
  input {
    width: 93%;
    height: 50px;
    margin-top: 30px;
    padding: 0 10px;
    background-color: #e6e6e6;
    font-family: "Press Start 2P";
    font-size: 20px;
  }
}

.bottom-modes {
  grid-row-start: 5;
  grid-row-start: 5;
  grid-column-start: 4;
  display: grid;
  grid-template-columns: 25% 25% 25% 25%;
  grid-template-rows: 40% 20% 40%;
}

.black-button {
  background-color: #404040;
  border: #303030 5px solid;
  color: gray;
  cursor: pointer;
}
.pokedex-mode {
  grid-row-start: 3;
  grid-column-start: 1;
  grid-column-end: 3;
}
.game-mode {
  grid-row-start: 3;
  grid-column-start: 1;
  grid-column-end: 5;
}

.level-button {
  background-color: #1697f9;
  border: #71cdf4 5px solid;
  color: gray;
  cursor: pointer;
}

</style>
