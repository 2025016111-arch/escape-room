<template>
  <div class="fail-screen">
    <h1>Você falhou!</h1>
    <p>O tempo acabou... a sala permaneceu trancada.</p>

    <img id="falha" src="/imagen/falha.png" alt="falho" />

    <button @click="$emit('restart')">Tentar novamente</button>
  </div>
</template>

<script>

export default { name: "Fail" } //exporta o componente fail para ser usado no App.vue

</script>

<style scoped>
.fail-screen{
  background:black;
  color: rgb(255, 80, 80);
  height:100vh;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  text-align:center;
}

.fail-screen h1 {
  font-size: 5rem; /* fonte bem grande */
  margin-bottom: 20px;
}

#falha {
  width:400px;
  margin:20px 0;
}

button{
  background: rgb(200, 40, 40);
  color:white;
  border:none;
  cursor:pointer;
  border-radius:6px;
  padding: 10px 20px;
  font-size: 1.2rem;
}
</style>
