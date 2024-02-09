<template>
  <div id="container">
    <h1>SMASH</h1>

    <div id="court">
      <button class="racket" id="racket_p1" @click="golpeP1">P1</button>
      <button class="racket" id="racket_p2" @click="golpeP2">P2</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      energy: 1,
      player_active: 1,
      last_player_golpe: null
    }
  },
  methods: {
    golpeP1() {
      if (this.last_player_golpe !== 1) {
        console.log("Golpe del P1");
        this.last_player_golpe = 1;
        this.player_active = 2;
        console.log("Jugador activo:", this.player_active);
        this.rollDice();
      }
    },
    golpeP2() {
      if (this.last_player_golpe !== 2) {
        console.log("Golpe del P2");
        this.last_player_golpe = 2;
        this.player_active = 1;
        console.log("Jugador activo:", this.player_active);
        this.rollDice();
      }
    },
    rollDice() {
      const diceValue = Math.floor(Math.random() * 6) + 1;
      console.log("Valor del dado:", diceValue);
    }
  }
}
</script>

<style scoped>
  /* Aquí van tus estilos CSS */
</style>


<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

  * {
    font-family: Roboto;
  }

  #container {
    background-color: #d6e8a3;
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  h1 {
    text-align: center;
    margin-bottom: 20px;
  }

  #court {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
    width: 300px;
    height: 150px;
    padding: 20px;
    border: 1px solid black;
    background-color: #ffffff;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  }

  .racket {
    width: 100%;
    height: 100%;
    font-size: 24px;
    border: none;
    background-color: #a3d6e8; /* Color verde similar a una cancha de tenis */
    color: #000000; /* Color del texto */
    border-radius: 10px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  .racket:hover {
    background-color: #89bdcf; /* Color verde más oscuro al pasar el ratón */
  }
</style>
