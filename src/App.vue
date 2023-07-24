<template>
 <div id="app">
   <h1>{{cepUsuario}}</h1>
   <input type="Text" v-model="cepUsuario"/>
   <button @click="pesquisar()">Pesquisar</button>
   
   <router-view></router-view>
   </div>
 <!--</div>-->
</template>

<script>
import axios from "axios"
export default {
  name: 'App',
  data(){
    return{
      cepUsuario:null,
      Pesquisar:true
    }
  },
  components: {
    
  },
  created(){
    this.obterInformaçoes();
  },
methods:{
  pesquisar(){ 
    this.obterInformaçoes(this.cepUsuario);
    },
  obterInformaçoes(cep){
    axios
    .get(`https://viacep.com.br/ws/${cep}/json/`)
    .then((resposta) => {
      if(resposta.data.localidade){
      console.log(resposta.data);
      alert(`A rua é:${resposta.data.localidade}`);
     }
    else{
       console.log(resposta.data);
       alert(`A rua não pertece ao Iporá e da cidade:${resposta.data.localidade}`);
     }
    });
  }
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
