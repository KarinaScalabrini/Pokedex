<template>
    <div class="cardPokemon">
        <img :src="pokemon.imagem" alt="imagem pokémon"/>
            <h3>{{ capitalizeFirstLetter(pokemon.name)}}</h3>
            <p>{{ formatarID(pokemon.id) }}</p>
            <div class="type">
                <span v-for="item in pokemon.type" :key="item.name" :class="getTypeButton(item)">{{capitalizeFirstLetter(item)}}</span>
            </div>
    </div>
    
</template>

<script>

export default {
    name: 'CardsPage',
    props: ['pokemon'],
    data(){
        return{
        }
    },
    methods:{
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
.cardPokemon{
    width:18%;
    min-width:197px;
    min-height: 264px;
    max-width:200px;
    max-height: 275px;
    max-width:270px;
    height:35vh;
    max-height:340px;
    background-color: rgba(13, 12, 12, 0.62);
    border-radius:15px;
    padding:1%;
    position:relative;
}
.cardPokemon:hover{
    box-shadow: 1px 1px 20px 1px #0540D9;
    background-color: rgb(1 5 18 / 92%);
}
  .cardPokemon > h3{
    color:#FEFFE2;
    font-family: 'Hind Siliguri', sans-serif;
    font-size:1.5rem;
    position:absolute;
    bottom:45px;
  }
  .cardPokemon > img{
    background-size: cover;
    background-repeat: no-repeat;
    width:100%;
    height:80%;
    overflow:hidden;
    transition: transform 0.3s;
  }
  .cardPokemon > p{
    font-size: 0.9rem;
    color: #d5d3d3;
    font-family: 'Hind Siliguri', sans-serif;
    margin-top: 1%;
  }
  .cardPokemon > img:hover{
    transform: scale(1.2);
  }
  .type{
    width:100%;
    display:flex;
    flex-direction: row;
    justify-content:start;
    position:absolute;
    bottom:10px;
  }
  .type > span{
    width:43%;
    height:25%;
    border-radius:8px;
    text-align: center;
    padding:1px;
    color:#DED9D9;
    font-size:1rem;
    
  }
  .type > span:nth-child(2){
    margin-left:4%;
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
    .cardPokemon{
        width:70%;
        padding:4%;
        position:relative;
    }
}
</style>
