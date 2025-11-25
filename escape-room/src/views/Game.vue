<template>
  <div id="room"> 

    <div id="timer">{{ tempoFormatado }}</div>

    <img id="porta" src="imagen/porta.png" @click="tryDoor" />
    <img v-show="!fechadura1Destrancada" id="fechadura1" src="imagen/fechadura1.png" @click="tryFechadura1" />
    <img v-show="!fechadura2Destrancada" id="fechadura2" src="imagen/fechadura2.png" @click="tryFechadura2" />
    <img v-show="!temChave" id="chave" src="imagen/chave.png" @click="pegarChave" />
    <img id="papel" src="imagen/papel.png" @click="pegarPapel" />

    <button id="dica" v-show="mostrarDicaButton" @click="mostrarDica">Dica</button>

    <button id="escapar" v-show="fechadura1Destrancada && fechadura2Destrancada" @click="finishGame"> Sair daqui </button>

  </div> 
</template> 

<script> //
export default {
  name: "Game", //define nome do componente
  data() {
    return {
     
      temChave: false, // o jogador já pegou a chave?
      fechadura1Destrancada: false, // a fechadura 1 já abriu?
      fechadura2Destrancada: false, // a fechadura 2 já abriu?
      tentativas: 0, // conta tentativas erradas da senha
      mostrarDicaButton: false, //mostra o botão dica

      time: 0, // conta os segundos
      interval: null // guarda o setInterval que atualiza o relógio

    } //fim return
  }, //fim data

  mounted() { // é chamado quando você entra na pagina game
    console.log('[Game] mounted');
    this.startTimer(); //chama a função startTimer() iniciando o cronômetro assim que o jogo começa
  },

  beforeUnmount() { // é chamado quando você sai da pagina
    this.stopTimer(); // para o cronômetro para evitar que continue rodando escondido
  },

  computed: { // define propriedades calculadas automaticamente sempre que seus valores mudam
    tempoFormatado() { //cria uma função reativa que formata o cronômetro
      const min = Math.floor(this.time / 60); // converte segundos em minutos
      const sec = this.time % 60; // o operador % pega o resto da divisão (ex. 73 seg = 1 min e 13 seg)
      return `${String(min).padStart(2,'0')}:${String(sec).padStart(2,'0')}`; // é o que transforma na forma que esta na tela mm:ss (ex. 01:40)

    } //fim tempoFormato
  }, //fim computed

  methods: { // parte principal 
   
   
    startTimer() { // função que inicia o cronometro
     
      if (this.interval) return; // evita duplicar o cronômetro (se interval já tem um timer dentro não cria outro)
      this.interval = setInterval(() => { // cria um "loop" automático
        this.time++; //incrementa a variável time do componente em 1 (+1 segundo)

       
        if (this.time >= 180) { // se passar de 3 min (180 seg) é falha automatica
          this.stopTimer(); // para de atualizar o timer
                    alert("O tempo acabou! Você não conseguiu escapar!");// mensagem antes de ir para fail
          this.$emit("fail"); // emite um evento chamado "fail" para o componente pai no App.vue
        } //fim if

      //1000 é milisegundo = 1 segundo, é o que faz o cronometro acontecer
      }, 1000); // fim setInterval

    }, //fim start timer

    stopTimer() { //função que para o cronometro
      if (this.interval) { // se existe um cronometro ativo
        clearInterval(this.interval); // remove da memoria
        this.interval = null; //"limpa" a variavel do cronometro
      } //fim if
    }, //fim stop timer

    finishGame() { // o fim do jogo
      this.stopTimer(); // chama para o cronometro
      this.$emit("finish", this.time); // envia o tempo para App.vue
    }, //finish game

    /* jogo */
    tryDoor() { // ve se a porta esta fechada
      alert("A porta está fechada. Destranque as fechaduras primeiro!");
    },//try door

    tryFechadura1() { // metodo para tentar abrir a fechadura 1
      const senha = prompt("Digite a senha:"); //pede ao jogador para digite a senha

      if (senha === "padre") { //se for a senha correta
        this.fechadura1Destrancada = true; // marca fechadura como destrancada e some com a fechadura
        alert("Fechadura destrancada!");
        this.mostrarDicaButton = false; // esconde o botão de dica
      } else { // se for a senha errada
        alert("Senha incorreta!");
        this.tentativas++; // incrementa o contador de tentativas erradas
        if (this.tentativas >= 3) { // se o jogador errou 3 vezes
          this.mostrarDicaButton = true; // o botão de dica aparece
          alert("Você errou 3 vezes — uma dica apareceu.");
        } //fim if

      } //fim else

    },// fim try fechadura 1

    tryFechadura2() { // metodo para tentar abrir a fechadura 2
      if (this.temChave) { // verifica se o jogador já pegou a chave
        this.fechadura2Destrancada = true; // se tiver a chave marca a fechadura 2 como destrancada
        alert("Fechadura destrancada!"); 
        this.mostrarDicaButton = false; // garante que o botão de dica fique escondido
      } else {
        alert("Você precisa da chave!");
      } //fim else
    }, //fim try fechadura 2

    pegarChave() { 
      this.temChave = true; // marca que pegou a chave e some com ela
      alert("Você pegou a chave!");
    }, //fim pegar chave

    pegarPapel() {
      alert("Enigma: Já casei muitas vezes, mas sempre continuo solteiro. Quem sou eu?");
    }, //fim papel

    mostrarDica() {
      alert("Dica: é alguém que usa bata.");
    } //fim dica

  }// fim methods
} // fim export defalts

</script>

<style scoped> /* as regras só serão aplicadas a este componente */

#room { /* css do container room */
  width: 100%; 
  height: 100vh;

  background-image: url('imagen/escape room.png');
  background-size: cover;
  background-position: center top;
  background-repeat: no-repeat;

  background-color: #000000; 
}

#timer { /* posição do cronometro */
  position: absolute;
  top: 20px;
  left: 20px;
  font-size: 2rem;
  color: white;
  background: rgba(0,0,0,0.5);
  padding: 8px 14px;
  border-radius: 8px;
}

#room > * { /* seleciona todos os filhos diretos de #room e os posiciona absolutamente */
  position: absolute;
}

#porta { top: 314px; left: 551px; } /* posição da porta */ 
#fechadura1 { top: 365px; left: 710px; } /* posição da fechadura 1*/
#fechadura2 { top: 430px; left: 694px; } /* posição da fechadura 2*/

#chave { /* posição da chave */
  top: 620px;
  left: 1000px;
  transform: rotate(45deg);
  cursor: pointer;
}
#papel { /* posição do papel com enigma */
  top: 640px;
  left: 373px;
  transform: rotate(-10deg);
  cursor: pointer;
}
#dica { /* posição do botão dica */
  width: 100px;
  height: 50px;
  top: 250px;
  left: 1000px;
}
#escapar { /* posição do botão sair daqui*/
  width: 153px;
  height: 60px;
  top: 486px;
  left: 578px;
}

button { /* estiliza todos os botões no componente */
  background: rgb(42, 132, 95);
  color: white;
  border: none;
  cursor: pointer;
}
</style>
