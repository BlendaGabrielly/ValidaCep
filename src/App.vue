<template>
 <div id="app">
   <h1>{{cepUsuario}}</h1>
   <input :class="{ 'input-erro': erro }" required type="Text" placeholder="Digite o cep" v-model="cepUsuario"/>
   <button :class="{ 'botao-clicado': botaoClicado }" @click="trocarCor(),pesquisar()">Pesquisar</button>
   <router-view></router-view>
   </div>

   <!-- Mostrar informações do CEP na página -->
<div v-if="informacoesCEP!== null">
  <p>O cep encontrado: {{ informacoesCEP.localidade }}</p>
</div>
<div v-else>
  <p>{{ mensagemErro }}</p>
</div>
</template>

<script>
import axios from "axios"
export default {
  name: 'App',
  data(){
    return{
      cepUsuario:null,
      Pesquisar:true,
      botaoClicado: false,
      mensagemErro: "", 
      erro: false, 
      informacoesCEP: null
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
        this.informacoesCEP = resposta.data;
        this.mensagemErro = "";
        this.erro = false;
     }
    else{
      this.informacoesCEP = null; 
      this.mensagemErro = "CEP não encontrado."; 
      this.erro = true;
      }
    })
        .catch((error) => {
          console.error("Erro nas informações do CEP:", error);
          this.informacoesCEP = null;
          this.mensagemErro = "Erro ao obter informações do CEP."; // Define a mensagem de erro em caso de erro na requisição
          this.erro = true;
        });
  },
  trocarCor() {
      // Altera o valor da propriedade botaoClicado para o oposto do seu valor atual
      this.botaoClicado = !this.botaoClicado;
    },
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
@keyframes shake{
  0%, 100% {translate: 0;}
  25% {translate: 8px 0;}
  75% {translate: -8px 0;}
}
input{
  width: 210px;
  height: 60px;
  padding: 0 16px;
  background: transparent;
  border-radius: 4px;
  color: #010000;
  animation: shake 0.14s 3;
}
input:valid{
  border-color: #45feaf;
  animation: none;
}
input:error{
  border-color: #db1414;
  animation: shake 0.14s 3;
}
button {
  background-color: #9932cc; /* Cor do botão (substitua pelo código da cor desejada) */
  color: #ffffff; 
  border-radius: 20% 50%;
  width: 210px;
  height: 60px;
}
.botao-clicado {
  background-color: #00ff00; /* Cor do botão quando clicado */
  color: #ffffff; /* Cor do texto no botão quando clicado */
}
</style>
