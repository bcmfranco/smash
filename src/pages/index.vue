<template>
  <div id="container">
    <h1 id="logo">SMASH</h1>

    <div id="pointer">
      <div class="player-score">{{ points.player_1 }}</div>
      <div class="player-score">{{ points.player_2 }}</div>
    </div>

    <div id="court" :style="{ backgroundColor: courtBackgroundColor }">
      <div id="mensenger">{{ this.energy }}</div>
      <div id="barr" :class="{ 'flex-row': player_active === 1, 'flex-row-reverse': player_active === 2 }">
        <div id="ball"></div>
      </div>
    </div>


    <div id="joystick">
      <div id="dice">
        <div>{{ dice }}</div>
      </div>

      <div id="controlers">
        <button class="racket" id="racket_p1" @click="shot(1)">P1</button>
        <button class="racket" id="racket_p2" @click="shot(2)">P2</button>
      </div>
    </div>

    <div id="buttoner">
      <div id="restart_btn" @click="restartMatch">Restart Match</div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      energy: 0,
      player_active: 1,
      points: {
        player_1: 0,
        player_2: 0,
      },
      fading_msg: null,
      fadding_2: null,
      showElement: true,
      diceRolling: false,
      dice: 1,
      courtBackgroundColor: '#f2f2f2', // Color de fondo inicial del #court,
    }
  },
  methods: {
    rollDice() {
      this.dice = Math.floor(Math.random() * 6) + 1;
      console.log("dice", this.dice);

      return this.dice;
    },
    getPoint(player){

      console.log("punto para", player);

      if(player === 1){
        this.points.player_1++;
      } else {
        this.points.player_2++;
      }

      console.log("marcador", this.points.player_1, " - ", this.points.player_2);
    },
    shot(player) {
      var shot_power = this.rollDice();


      if(player === 1){
        var anti_player = 2;
        this.player_active = 2;
      } else {
        var anti_player = 1;
        this.player_active = 1;
      }

      console.log("energy: ",this.energy, "power:", shot_power);

      if(shot_power > this.energy){
        console.log("hi");
        this.energy = shot_power - this.energy;

        console.log("new energy: ",this.energy);
      } else {
        console.log("hao");
        this.energy = 0;
        this.courtBackgroundColor = '#ff6666'; // Cambiar color de fondo en caso de 'Fail'
        setTimeout(() => {
          this.courtBackgroundColor = '#f2f2f2'; // Restaurar color de fondo después de 1 segundo
        }, 1000);

        this.getPoint(anti_player);
      }
    },
    restartMatch() {
      // Reinicia el puntaje y el estado del juego
      this.points.player_1 = 0;
      this.points.player_2 = 0;
      this.energy = 0;
      this.player_active = 1;
      this.courtBackgroundColor = '#ff6666'; // Cambiar color de fondo en caso de 'Fail'
      setTimeout(() => {
        this.courtBackgroundColor = '#f2f2f2'; // Restaurar color de fondo después de 1 segundo
      }, 1000);
      
      console.log("marcador", this.points.player_1, " - ", this.points.player_2);
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
    box-sizing: border-box;
  }

  #container {
    background-color: #f5f5f5; /* Cambié el color de fondo */
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  h1#logo {
    font-family: 'Montserrat', sans-serif; /* Cambié la fuente a Montserrat */
    font-size: 64px; /* Aumenté ligeramente el tamaño */
    font-weight: 700; /* Aumenté la intensidad del texto */
    color: #333333;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
    letter-spacing: 2px; /* Aumenté el espaciado entre letras */
  }

  #joystick {
    width: 300px;
    height: 200px;
    border: 2px solid #dddddd; /* Suavicé el color del borde */
    background-color: #ffffff;
    border-radius: 15px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Añadí una sombra suave */
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .points {
    font-size: 24px;
    border: none;
    background-color: #e0e0e0; /* Suavicé el color de fondo */
    color: #333333;
    border-radius: 10px;
    text-align: center;
    display: grid;
    align-items: center;
    padding: 10px;
  }

  #controlers {
    display: flex;
    gap: 10px;
    justify-content: center;
    align-items: center;
  }

  .racket {
    width: 80px;
    height: 80px;
    font-size: 24px;
    border: none;
    background-color: #4caf50; /* Cambié el color del botón */
    color: #ffffff;
    border-radius: 50%;
    cursor: pointer;
    transition: background-color 0.3s ease;
    display: flex;
    justify-content: center;
    align-items: center;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Añadí una sombra suave */
  }

  .racket:hover {
    background-color: #43a047; /* Cambié el color al pasar el ratón */
  }

  #buttoner {
    margin-top: 20px;
  }

  #restart_btn {
    padding: 10px 20px;
    font-size: 18px;
    background-color: #f44336; /* Cambié el color del botón */
    color: #ffffff;
    border: none;
    border-radius: 10px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Añadí una sombra suave */
  }

  #restart_btn:hover {
    background-color: #e53935; /* Cambié el color al pasar el ratón */
  }

  #court {
    border: 2px solid #cccccc; /* Cambié el color del borde */
    width: 400px;
    height: 150px;
    margin: 10px auto 20px auto;
    border-radius: 15px;
    display: grid;
    justify-items: center;
    align-items: center;
    transition: background-color 1s ease;
    background-color: #ffffff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Añadí una sombra suave */
  }

  #barr {
    width: 300px;
    height: 30px;
    background-color: #eeeeee; /* Suavicé el color de fondo */
    display: flex;
    align-items: center;
    border-radius: 15px;
    overflow: hidden;
  }

  #barr #ball {
    height: 100%;
    width: 30px;
    background-color: #f44336; /* Cambié el color del punto */
  }

  .flex-row {
    display: flex;
    flex-direction: row;
  }

  .flex-row-reverse {
    display: flex;
    flex-direction: row-reverse;
  }

  #dice {
    font-size: 48px;
    font-weight: bold;
    color: #ffffff;
    background-color: #9e9e9e; /* Cambié el color de fondo */
    width: 80px;
    height: 80px;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1); /* Suavicé la sombra */
    margin: 10px 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Añadí una sombra suave */
  }

  .dice-roll {
    transform: scale(1.5);
  }

  #pointer {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }

  .player-score {
    width: 50px;
    height: 50px;
    border: 2px solid #333333; /* Cambié el color del borde */
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: bold;
    margin: 0 10px;
    background-color: #ffffff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Añadí una sombra suave */
  }
</style>


