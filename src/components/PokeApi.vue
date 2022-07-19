<template>
  <div>
    <img src="../assets/logo.jpg" alt="logo">
    <h2>PokeGuía</h2>
    <form>
      <span>Nombre:</span>
      <input type="text" v-model="texto" @keydown.enter.prevent="obtener" placeholder="Ingrese un pokemon">
      <button @click.prevent="obtener">Buscar</button>
    </form>
    <img :src="foto" alt="img">
    <h3>Movimientos</h3>
    <div class="contenedor">
      <ul v-for="(movimiento, i) in movimientos" :key="i">
        <li>{{movimiento.move.name}}</li>
      </ul>
    </div>
    <h3>Habilidades</h3>
    <div class="contenedor"> 
      <ul v-for="(habilidad, i) in habilidades" :key="i">
        <li>{{habilidad.ability.name}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokeApi',
  // DATA
  data: function(){
    return {
      texto:'',
      pokemon:'pikachu',
      jsonPokemon:''
    }
  },
  // COMPUTED
  computed:{
    foto(){
      let {sprites} = this.jsonPokemon;
      if(sprites == undefined){
        return 'https://i.giphy.com/media/y1ZBcOGOOtlpC/giphy.gif';
      }
      return sprites.front_default;
    },
    movimientos(){
      let {moves} = this.jsonPokemon;
      return moves;

    },
    habilidades(){
      let {abilities} = this.jsonPokemon;
      return abilities;
    }
  },
  // METHODS
  methods:{
    obtener: function () {
      let texto = this.texto;
      (texto != '')? this.pokemon = texto : alert('Ingrese un pokémon')
      texto = ''
      this.fetchPokeApi()
    },
    async fetchPokeApi(){
      try {
        let data = await fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemon}`)
        let json = await data.json()
        if(data.ok){
          this.jsonPokemon = json;
        }
      } catch (error) {
        alert(error)
      }
    }
  },
  // CREATED
  created: function(){
    this.fetchPokeApi()
  }
}
</script>

<style scoped>
.contenedor {
  width: 40%;
  margin: auto;
  text-align: center;
}
h3{
  font-size: 1.5rem;
}
h2 {
  font-size: 2rem;
  font-weight: bold;
}
button {
  border: 1px solid rgb(208, 208, 208);
  background: #fff;
  cursor:pointer;
  transition: 300ms
}
button:hover {
  background: rgb(77, 180, 236);
  color: #fff;
}
li {
  list-style:none;
}
</style>
