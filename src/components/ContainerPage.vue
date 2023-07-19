<template>
    <div class="Container">
        <div class="input-group mb-3 pesquisa">
            <input type="text" v-model="pesquisa" class="form-control inputPesquisa" placeholder="Name or number" aria-label="Search Pokémon" aria-describedby="search-pokemon">
            <button @click="searchPokemon" class="btn btn-outline-secondary" type="button" id="search-pokemon">
              <span>SEARCH</span>
              <span class="bi bi-search"></span>
            </button>
        </div>
        <div class="Container2" v-if="dadosCarregados">
            <CardsPage  v-for="pokemon in pokemonInfoArray" :key="pokemon.id" :pokemon="pokemon" @click="abrirModal(pokemon)" aria-label="Pokémon" type="button"  data-bs-toggle="modal" data-bs-target="#modalPokemon"/>
        </div>
        <p v-else>Carregando...</p>
        <div class="spinner-border text-primary" v-if="load" role="status">
            <span class="visually-hidden">Loading...</span>
        </div>
        <button v-else class="btn-load"  type="button" @click="loadPokemons" aria-label="Load more Pokémon" id="loadingPokemons">LOAD MORE POKÉMON</button>
        <div class="modal fade" id="modalPokemon" tabindex="-1" :caracteristicas="caracteristicaModal" aria-labelledby="modalPokemon" v-show="mostrarModal">
                <div class="modal-dialog modal-dialog-centered justify-content-center ">
                    <div class="modal-content">
                    <div class="modal-body">
                      <div class="aboutPokemon">
                        <div>
                          <p>Height:<strong>{{ (caracteristicaModal.altura / 10).toFixed(1) }} Mt</strong></p>
                          <p>Weight:<strong>{{(caracteristicaModal.peso / 10).toFixed(1) }}Kg</strong></p>
                          <div >Abilities:<strong v-for=" habilidade in caracteristicaModal.habilidades" :key="habilidade.id">{{capitalizeFirstLetter(habilidade.name)}}</strong></div>
                          <div class="tipo">Type:<span v-for=" tipo in caracteristicaModal.type" :key="tipo.id" :class="getTypeButton(tipo)">{{capitalizeFirstLetter(tipo)}}</span></div>
                          <h1>{{ capitalizeFirstLetter(caracteristicaModal.name)}}</h1>
                          <p>{{ formatarID(caracteristicaModal.id) }} </p>
                        </div>
                        <img :src="caracteristicaModal.imagem"/>
                      </div>
                        <div class="hability">
                            <div v-for="(caracteristica, index) in caracteristicaModal.estatisticas" :key="index">
                              <span>{{ capitalizeFirstLetter(caracteristica.name) }}</span><div class="porcentagemDiv" :style="{ width: (caracteristica.base_stat / 150) *100 + '%' }"></div>
                            </div>
                        </div>
                    </div>
                        
                    </div>
                    </div>
                </div>
    </div>
  </template>
  
  <script>
    import CardsPage from './CardsPage.vue'

  export default {
    name: 'ContainerPage',
    components:{
        CardsPage
    },
    data(){
        return{
            pokemonInfoArray:[],
            dadosCarregados: false,
            load: false,
            pesquisa: "",
            caracteristicaModal: {},
            mostrarModal: false,

        }
    },
    async beforeMount() {
        await this.carregarPokemons("loadPokemon");
    },
    methods:{
        async carregarPokemons() {
            try {
                const response = await fetch('https://pokeapi.co/api/v2/pokemon/?limit=20');
                const data = await response.json();
                const cards = data.results;
                this.pokemonInfoArray = await Promise.all(cards.map(async (pokemon) => {
                const response = await fetch(pokemon.url);
                const pokemonData = await response.json();
                const typesArray = pokemonData.types.map((typeInfo) => typeInfo.type.name);

                const statsDetails = pokemonData.stats.map((stat) => {
                    return {
                        base_stat: stat.base_stat,
                        name: stat.stat.name
                    };
                });
                const ability = pokemonData.abilities.map((ability) => {
                    return {
                        name: ability.ability.name
                    };
                });
          
                return {
                    name: pokemon.name,
                    id: pokemonData.id,
                    type: typesArray,
                    imagem: pokemonData.sprites.front_default,
                    altura: pokemonData.height,
                    peso: pokemonData.weight,
                    habilidades: ability,
                    estatisticas: statsDetails
                };
                }));
                this.dadosCarregados = true;
            } catch (error) {
                console.error('Erro ao carregar os dados:', error);
            }
    },
    async loadPokemons() {
      this.load = true;
      try {
        const nextResponse = await fetch('https://pokeapi.co/api/v2/pokemon/?offset=20&limit=20');
        const nextData = await nextResponse.json();
        const nextCards = nextData.results;
        const newPokemons = await Promise.all(nextCards.map(async (pokemon) => {
          const response = await fetch(pokemon.url);
          const pokemonData = await response.json();
          const typesArray = pokemonData.types.map((typeInfo) => typeInfo.type.name);
          const statsDetails = pokemonData.stats.map((stat) => {
                    return {
                        base_stat: stat.base_stat,
                        name: stat.stat.name
                    };
                });
                const ability = pokemonData.abilities.map((ability) => {
                    return {
                        name: ability.ability.name
                    };
                });
          return {
                    name: pokemon.name,
                    id: pokemonData.id,
                    type: typesArray,
                    imagem: pokemonData.sprites.front_default,
                    altura: pokemonData.height,
                    peso: pokemonData.weight,
                    habilidades: ability,
                    estatisticas: statsDetails
                };
        }));


        this.pokemonInfoArray = this.pokemonInfoArray.concat(newPokemons);
        this.load = false;
      } catch (error) {
        console.error('Erro ao carregar mais dados:', error);
      }
    },
    async searchPokemon() {
      if (!this.pesquisa) {
        return;
      }

      try {
        const response = await fetch(`http://pokeapi.co/api/v2/pokemon/${this.pesquisa.toLowerCase()}`);
        const pokemonData = await response.json();
        const typesArray = pokemonData.types.map((typeInfo) => typeInfo.type.name);

        this.pokemonInfoArray = [];
        const statsDetails = pokemonData.stats.map((stat) => {
          return {
            base_stat: stat.base_stat,
            name: stat.stat.name
          };
        });
        const ability = pokemonData.abilities.map((ability) => {
          return {
            name: ability.ability.name
          };
        });

        this.pokemonInfoArray.push({
          name: pokemonData.name,
          id: pokemonData.id,
          type: typesArray,
          imagem: pokemonData.sprites.front_default,
          altura: pokemonData.height,
          peso: pokemonData.weight,
          habilidades: ability,
          estatisticas: statsDetails
        });

        this.dadosCarregados = true;
      } catch (error) {
        console.error('Erro ao pesquisar o Pokémon:', error);
      }
    },
    abrirModal(pokemon) {
      this.caracteristicaModal = pokemon;
      this.mostrarModal = true;
    },
    getTypeButton(type){
            const typeClass = {
                normal: 'normal',
                fighting: 'fighting',
                flying: 'flying',
                poison: 'poison',
                ground: 'ground',
                rock: 'rock',
                bug: 'bug',
                ghost: 'ghost',
                steel: 'steel',
                fire: 'fire',
                water: 'water',
                grass: 'grass',
                electric: 'electric',
                psychic: 'psychic',
                ice: 'ice',
                dragon: 'dragon',
                dark: 'dark',
                fairy: 'fairy',
                unknow: 'unknow',
                shadow: 'shadow',
            };
            return typeClass[type] || 'default-button';

    },
    capitalizeFirstLetter(str) {
            if (!str) return '';
            return str.charAt(0).toUpperCase() + str.slice(1);
    },
    formatarID(id) {
      const numeroZeros = id < 10 ? '000' : id < 100 ? '00' : id < 1000 ? '0' : '';
      return `#${numeroZeros}${id}`;
    }
  }
  }
  </script>
  
  <style scoped>
  .Container{
    display:flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-bottom:5%;
    width:85%;
    margin:auto;
    padding-left:4%;
    padding-right:4%;
    
  }
  .Container2{
    display:flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center ;
    gap:1.5rem;
  }
  .pesquisa{
    width:60%;
    height:5vh;
    margin-bottom:3%!important;
  }
  .pesquisa> button{
    width:20%;
    border-radius:10px!important;
    background-color: #C6C2C2;
    color:#282424;
    font-weight:700;
  }
  .inputPesquisa{
    background-color:rgba(13, 12, 12, 0.62)!important;
    margin-right: 3%;
    border-radius:10px!important;
    border:1px solid #0540D9;
    color:#FEFFE2!important;
    font-size:14px!important;
  }
  .btn-load{
    padding:1% 2%;
    background-color: #0540D9;
    color:#FEFFE2;
    font-family: 'Hind Siliguri', sans-serif;
    font-size:14px;
    border-radius:9px;
    border:none;
    margin-top:5%;
  }
  .porcentagemDiv {
  background-color: #0540D9;
  color: #ddddd7;
  height: 10px;
  text-align: center;
  line-height: 30px;
  border-radius: 5px;
}
.hability{
  width:90%;
  margin:auto;
}
.aboutPokemon{
  width:90%;
  margin:auto;
  height:60%;
  display:flex;
  flex-direction:row;
  justify-content: space-between;
  align-items: center;
}
.aboutPokemon > div {
  width:50%;
  height:100%;
  
}
.aboutPokemon > div > h1 {
  color:#ddddd7;
  font-family: 'Hind Siliguri', sans-serif;
  font-size:2.5rem;
  margin-top:9%;

}
.aboutPokemon > div > p, div {
  color:#ddddd7;
  font-family: 'Hind Siliguri', sans-serif;
  font-size:1.2rem;
  margin:0px;
  margin-bottom:1%;

}
.aboutPokemon > div > p > strong {
  color:#0540D9;
  font-family: 'Hind Siliguri', sans-serif;
  font-size:1.5rem;
  margin:0% 0% 2% 4%;

}
.aboutPokemon > div > div > strong {
  color:#0540D9;
  font-family: 'Hind Siliguri', sans-serif;
  font-size:1.5rem;
  margin:0% 2% 2% 4%;

}
.aboutPokemon > img{
  width:50%;
  height:100%;
}
.modal-content{
  background-color: rgb(0 3 12)!important;
  margin:auto!important;
  padding:4%;
  width:50vw!important;
  box-shadow:1px 1px 20px 1px #01162f;
}
.modal-dialog{
  margin:auto!important;
}
.tipo > span{
    width:78px;
    height:28px;
    border-radius:8px;
    text-align: center;
    padding:1px 16px;
    color:#DED9D9;
    font-size:1rem;
    margin-left:2%
    
  }
.bi-search{
  display:none;
}
.grass{
    background-color: #41713A;
}
.fire{
    background-color: #B74402;
}
.electric{
    background-color: #8F7503;
}
.poison{
    background-color: #7552BE;
}
.normal{
    background-color:#424040 ;
}
.fighting{
    background-color:#774e02 ;
}
.flying{
    background-color:#3e5f69 ;
}
.ground{
    background-color:#9e8749 ;
}
.rock{
    background-color:#2e2b2b ;
}
.bug{
    background-color:#330202 ;
}
.ghost{
    background-color:#0f0f0f ;
}
.steel{
    background-color:#414141 ;
}
.water{
    background-color:#033a68 ;
}
.psychic{
    background-color:#943284 ;
}
.ice{
    background-color:#0198a3 ;
}
.dragon{
    background-color:#b87004 ;
}
.dark{
    background-color:#0c1a09 ;
}
.fairy{
    background-color:#705375 ;
}
.unknow{
    background-color:#cac7c7 ;
}
.shadow{
    background-color:#463535 ;
}
@media screen and (max-width:750px){
  .pesquisa{
    width:95%;
    height:6vh;
    max-height:45px;
    margin-bottom:6%!important;
  }
  .pesquisa> button{
    width:20%;
  }
  .btn-load{
    padding:3% 8%;
    font-size:14px;
    margin-top:8%;
  }
  .aboutPokemon{
  width:90%;
  height:60%;
  display:flex;
  flex-direction:column-reverse;
  justify-content: center;
}
.aboutPokemon > div {
  width:100%;
  height:50%;
  font-size:0.9rem;
  
}
.aboutPokemon > div > h1 {
  font-size:2rem;

}
.aboutPokemon > div > p > strong {
  font-size:0.9rem;

}
.aboutPokemon > div > p {
  font-size:0.9rem;

}
.aboutPokemon > div > strong {
  font-size:0.9rem;

}
.aboutPokemon > div > div > strong {
  font-size:0.9rem;

}
.aboutPokemon > div > div {
  font-size:0.9rem;

}
.aboutPokemon > img{
  width:100%;
  height:50%;
}
.modal-content{
  width:80vw!important;
}
.modal-body{
  padding:0px!important;
}
.tipo{
  margin-top:2%;
}
.tipo > span{
    font-size:0.9rem;
  }
  .hability > div > span{
    font-size:0.9rem;
  }
 
  .pesquisa > button > span:nth-child(1){
    display:none;
  }
  .bi-search{
    display:flex;
    width:100%;
    height:100%;
    font-size:1.5rem;
    color:#0f0f0f;
  }
}
  </style>