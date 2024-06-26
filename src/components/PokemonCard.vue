<template>
  <div :class="['card', primaryTypeClass]">
    <div id="title-heading">
      <div id="name">{{ pokemonName }}</div>
      <div id="level">{{ hp }} HP</div>
    </div>
    <div id="pokemon-container">
      <img :src="pokemonImage" alt="Pokémon Image" id="poke-image"/>
    </div>
    <div id="banner" :class="primaryTypeClass">Type: {{ pokemonTypes.join(', ') }}</div>
    <div id="stats">
      <ul id="moves">
        <li v-for="move in moves" :key="move.name" :class="move.typeClass">{{ move.name }}</li>
      </ul>
    </div>
    <div id="footer">
      <button @click="fetchPokemon">New Pokémon</button>
      <button @click="changeGeneration">Change Generation</button>
    </div>
    

  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'PokemonCard',
  data() {
    return {
      pokemonName: '',
      pokemonImage: '',
      hp: 0,
      pokemonTypes: [],
      primaryTypeClass: '',
      moves: [],
      currentGeneration: 1
    };
  },
  mounted() {
    this.fetchPokemon();
  },
  methods: {
    async fetchPokemon() {
      const pokemonNumber = this.getPokemonNumberByGeneration(this.currentGeneration);
      try {
        const response = await axios.get(`https://pokeapi.co/api/v2/pokemon/${pokemonNumber}`);
        const pokemonData = response.data;

        this.pokemonName = pokemonData.name;
        this.pokemonImage = pokemonData.sprites.front_default;
        this.hp = pokemonData.stats.find(stat => stat.stat.name === 'hp').base_stat;
        this.pokemonTypes = pokemonData.types.map(typeInfo => typeInfo.type.name);

        // Set the primary type class for styling
        this.primaryTypeClass = this.getBackgroundColorClass(this.pokemonTypes[0]);

        // Fetch random moves and their types
        this.moves = await this.getRandomMoves(pokemonData);
      } catch (error) {
        console.error('Fetch error: ', error);
      }
    },
    getPokemonNumberByGeneration(generation) {
      switch (generation) {
        case 1:
          return Math.floor(Math.random() * 151) + 1;
        case 2:
          return Math.floor(Math.random() * 100) + 152;
        case 3:
          return Math.floor(Math.random() * 135) + 252;
        case 4:
          return Math.floor(Math.random() * 107) + 387;
        case 5:
          return Math.floor(Math.random() * 156) + 494;
        case 6:
          return Math.floor(Math.random() * 72) + 650;
        case 7:
          return Math.floor(Math.random() * 88) + 722;
        case 8:
          return Math.floor(Math.random() * 89) + 810;
        case 9:
          return Math.floor(Math.random() * 82) + 899;
        default:
          return Math.floor(Math.random() * 151) + 1;
      }
    },
    async getRandomMoves(pokemonData) {
      const possibleMoves = Math.min(pokemonData.moves.length, 4);
      const moves = [];

      function getRandomInt(min, max) {
        return Math.floor(Math.random() * (max - min)) + min;
      }

      for (let i = 0; i < possibleMoves; i++) {
        let moveFound = false;
        let randomIndex;
        let moveName;

        do {
          randomIndex = getRandomInt(0, pokemonData.moves.length);
          moveName = pokemonData.moves[randomIndex].move.name;
          moveFound = moves.some(move => move.name === moveName);
        } while (moveFound);

        const moveUrl = pokemonData.moves[randomIndex].move.url;
        const moveResponse = await axios.get(moveUrl);
        const moveData = moveResponse.data;
        moves.push({
          name: moveData.name,
          typeClass: this.getBackgroundColorClass(moveData.type.name)
        });
      }

      return moves;
    },
    changeGeneration() {
      this.currentGeneration = (this.currentGeneration % 9) + 1;
      this.fetchPokemon();
    },
    getBackgroundColorClass(type) {
      switch (type.toLowerCase()) {
        case 'dark':
          return 'dark';
        case 'flying':
          return 'flying';
        case 'fairy':
          return 'fairy';
        case 'fire':
          return 'fire';
        case 'steel':
          return 'steel';
        case 'normal':
          return 'normal';
        case 'ground':
          return 'ground';
        case 'rock':
          return 'rock';
        case 'grass':
          return 'grass';
        case 'poison':
          return 'poison';
        case 'psychic':
          return 'psychic';
        case 'dragon':
          return 'dragon';
        case 'water':
          return 'water';
        case 'ice':
          return 'ice';
        case 'bug':
          return 'bug';
        case 'electric':
          return 'electric';
        case 'fighting':
          return 'fighting';
        case 'ghost':
          return 'ghost';
        default:
          return 'default';
      }
    }
  }
};
</script>

<style>

.card {
  transform: scale(0.9);
  background-color: #E7BA77;
  background-repeat: none;
  background-size: cover;
  border: #FFE164 10px solid;
  border-radius: 10px;
  -moz-box-shadow: inset 0 0 10px #000000;
  -webkit-box-shadow: inset 0 0 10px #000000;
  box-shadow: inset 0 0 10px #000000;
  margin-top: 50px;
  margin-left: 10px;
  width: 390px;
}

#pokemon-container {
  background: linear-gradient(#186ED1, #44A4E1, #95CDEB);
  border: #FFE164 4px solid;
  border-style: outset;
  box-shadow: #301E0A 3px 3px 20px 2px;
  margin: 20px;
  height: 20%;
  padding-left: 20px;
  padding-top: 20px;
}

#poke-image {
  display: block;
  margin: 0 auto;
  width: 200px;
  height: 200px;
  border-radius: 8px;
  position: relative;
}

#title-heading {
  margin-left: 20px;
  margin-right: 20px;
  font-size: 0;
  position: relative;
  top: 20px; /* Reduce the top space */
  display: flex;
  justify-content: space-between; /* Add this to space out the name and level */
}

#name {
  color: white;
  font-family: 'Yanone Kaffeesatz', sans-serif;
  font-size: 25px;
  font-weight: 700;
  margin-right: auto; /* Align to the right */
}

#level {
  color: white;
  font-family: 'Josefin Sans', sans-serif;
  font-size: 25px;
  letter-spacing: -1px;
  text-align: right;
  margin-left: auto; /* Align to the left */
}


#banner {
  color: #fff5ce;
  border-radius: 5px;
  border: 2px #FFF883 solid;
  font-family: 'Oswald', sans-serif;
  font-size: 14px;
  font-style: oblique;
  margin: 0 auto;
  position: relative;
  text-align: center;
  bottom: 10px;
  width: 230px;
}

#stats table {
  border-collapse: collapse;
  height: 164px;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  width: 275px;
}

#footer {
  font-family: 'Yanone Kaffeesatz', sans-serif;
  font-size: 12px;
  font-style: oblique;
  font-weight: 500;
  margin: 0 auto;
  padding: 3px;
  position: relative;
  width: 260px;
  bottom: 20px;
  text-align: center;
}

#moves {
  list-style-type: none;
  padding: 0;
  transform: scale(0.95);
}

#moves li {
  font-family: 'Josefin Sans', sans-serif;
  color: white;
  border: 1px solid #FF9900;
  border-radius: 5px;
  margin: 5px 0;
  padding: 5px;
  text-align: center;
}

button {
  background-color: #FFCC00;
  border: none;
  border-radius: 5px;
  color: #000;
  cursor: pointer;
  font-family: 'Josefin Sans', sans-serif;
  font-size: 16px;
  margin: 5px;
  padding: 10px 20px;
}



button:hover {
  background-color: #FF9900;
}



.dark {
  background-color: rgb(0, 0, 0);
}

.flying {
  background-color: rgb(230, 204, 153); 
}

.fairy {
  background-color: rgb(255, 192, 203);
}

.steel {
  background-color: rgba(169, 169, 169); 
}

.normal {
  background-color: rgb(203, 202, 202); 
}

.ground {
  background-color: rgba(139, 69, 19); 
}

.rock {
  background-color: rgba(101, 67, 33); 
}

.grass {
  background-color: rgba(0, 128, 0); 
}

.poison {
  background-color: rgb(150, 1, 150);
}

.psychic {
  background-color: rgba(218, 112, 214); 
}

.dragon {
  background-color: rgba(0, 0, 255); 
}

.water {
  background-color: rgba(0, 81, 255);
}

.ice {
  background-color: rgba(173, 216, 230); 
}

.electric {
  background-color: rgb(201, 201, 46); /* Light Yellow */
}

.bug {
  background-color: rgba(77, 185, 77); /* Light Green */
}

.fire {
  background-color: rgba(255, 0, 0); /* Red */
}

.fighting {
  background-color: rgba(255, 165, 0); /* Orange */
}

.ghost {
  background-color: rgb(80, 1, 80); /* Different Purple */
}
</style>
