<template>
  <div class="center-screen">
    <h1>Bem-vindo ao Escape Room!</h1>
    <p>Seu objetivo é escapar da sala resolvendo enigmas e encontrando pistas.</p>
    <p>Você tem 3 minutos para escapar! </p>
    <p>Boa sorte!</p>
    <button @click="$emit('start')">Iniciar o jogo</button>
  </div>
</template>

<script>
export default { name: "Home" } //exporta o componente home para ser usado no App.vue
</script>

<style scoped>
.center-screen{
  background:black;
  color:rgb(255, 255, 255);
  font-family: Arial, sans-serif;
  height:100vh;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  text-align:center;
}
button{
  margin-top:20px;
  font-size:1.2rem;
  padding:0.6rem 1.2rem;
  background: rgb(41,168,115);
  color:white;
  border:none;
  cursor:pointer;
  border-radius:6px;
}
</style>
