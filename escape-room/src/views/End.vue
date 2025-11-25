<template>
  <div class="end-screen">
    <h1>Parabéns! Você escapou!</h1>

    <p>Seu tempo foi: <strong>{{ tempoFormatado }}</strong></p>

    <p>Obrigado por jogar meu Escape Room.</p>
    <p>Espero que tenha se divertido!</p>

    <img id="joia" src="/imagen/joinha.png" alt="joinha" />

    <button @click="$emit('restart')">Jogar novamente</button>
  </div>
</template>

<script>
export default { //exporta o objeto do componente
  name: "End", // define o nome do componente
  props: {// valor que vêm do componente pai
    tempo: {// recebe uma prop chamada tempo
      type: Number,// esta prop é um número
      required: true// é obrigatório o pai passar esse valor
    }//fim tempo
  },//fim props
  
  computed: { // define propriedades calculadas automaticamente sempre que seus valores mudam
    tempoFormatado() { //cria uma função reativa que formata o cronômetro
      const min = Math.floor(this.tempo / 60); // converte segundos em minutos
      const sec = this.tempo % 60; // o operador % pega o resto da divisão (ex. 73 seg = 1 min e 13 seg)
      return `${String(min).padStart(2,'0')}:${String(sec).padStart(2,'0')}`; // é o que transforma na forma que esta na tela mm:ss (ex. 01:40)
    
    }//fim tempoFormato
  
  } //fim computed

}//fim export default

</script>

<style scoped> /* as regras só serão aplicadas a este componente */

.end-screen{
  background:black;
  color: rgb(247, 223, 13);
  height:100vh;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  text-align:center;
}
#joia{
  width:350px;
  margin:20px 0;
}
button{
  font-size:1.2rem;
  padding:0.6rem 1.2rem;
  background: rgb(41,168,115);
  color:white;
  border:none;
  cursor:pointer;
  border-radius:6px;
}
</style>
