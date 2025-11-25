<template>
  <Home 
    v-if="pagina === 'home'" 
    @start="pagina='jogo'" 
  />

  <Game 
    v-if="pagina === 'jogo'" 
    @finish="endGame"
    @fail="pagina='fail'"
  />

  <End 
    v-if="pagina === 'fim'" 
    :tempo="tempoFinal"
    @restart="pagina='home'" 
  />

  <Fail 
    v-if="pagina === 'fail'" 
    @restart="pagina='home'" 
  />
</template>

<script>
//traz para dentro do App.vue os 4 componentes da aplicação
import Home from './views/Home.vue' //Home.vue = tela inicial
import Game from './views/Game.vue' // Game.vue = o jogo em si
import End from './views/End.vue' //End.vue = aparece quando o jogador vence
import Fail from './views/Fail.vue'  //Fail.vue = aparece quando o tempo acaba

export default {
  name: 'App',
  components: { Home, Game, End, Fail },
  data() { //define variáveis reativas que controlam tudo
    return {
      pagina: 'home', // diz qual tela deve aparecer no momento
      tempoFinal: 0 // recebe o tempo que o jogador levou para vencer
    } //fim return
  }, //fim data

  methods: {
    endGame(tempo) { //é chamado pelo Game.vue quando o jogador termina o jogo com sucesso
      this.tempoFinal = tempo; //salva o tempo final
      this.pagina = 'fim'; // troca para a página final
    } // fim endGame

  } //fim methods

} // fim export default

</script>
