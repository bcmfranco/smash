<template>
  <div id="container">
    <h1 id="logo">SMASH</h1>


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
      last_player_golpe: null,
      points: {
        player_1: 0,
        player_2: 0,
      },
      fading_msg: null,
      showElement: true,
      diceRolling: false,
      dice: 1
    }
  },
  methods: {
    rollDice() {
      this.dice = Math.floor(Math.random() * 6) + 1;
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

      console.log("energy: ",this.energy, "power:", shot_power);

      if(shot_power > this.energy){
        console.log("hi");
        this.energy = shot_power - this.energy;
        console.log("new energy: ",this.energy);
      } else {
        console.log("hao");
        this.energy = 0;

        if(player === 1){
          var anti_player = 2;
        } else {
          var anti_player = 1;
        }

        this.getPoint(anti_player);
      }
    },
    restartMatch() {
      // Reinicia el puntaje y el estado del juego
      this.points.player_1 = 0;
      this.points.player_2 = 0;
      this.energy = 0;

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
}

#container {
  background-color: #f2f2f2; /* Cambié el color de fondo para un aspecto más claro */
  height: 100vh;
  width: 100vw;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

h1#logo {
  font-size: 48px;
  font-weight: bold;
  color: #ffffff; /* Color blanco */
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* Sombra de texto */
  margin-bottom: 20px; /* Espacio inferior */
}


#controlers {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  width: 300px;
  height: 150px;
  padding: 20px;
  border: 1px solid #cccccc; /* Cambié el color del borde */
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Reduje la opacidad de la sombra */
}

.points {
  font-size: 24px;
  border: none;
  background-color: #e6e6e6; /* Cambié el color de fondo */
  color: #333333; /* Cambié el color del texto */
  border-radius: 10px;
  text-align: center;
  display: grid;
  align-items: center;
}

.racket {
  width: 100%;
  font-size: 24px;
  border: none;
  background-color: #66cc66; /* Cambié el color del botón */
  color: #ffffff; /* Cambié el color del texto */
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.racket:hover {
  background-color: #4CAF50; /* Cambié el color al pasar el ratón */
}

#buttoner {
  margin-top: 20px;
}

#restart_btn {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #ff6666; /* Color rojo */
  color: #ffffff; /* Color del texto */
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

#restart_btn:hover {
  background-color: #ff3333; /* Color rojo más oscuro al pasar el ratón */
}

#court {
  border: 2px solid #999999; /* Cambié el color del borde */
  width: 400px;
  height: 150px;
  margin: 10px auto 20px auto;
  border-radius: 15px;
  display: grid;
  justify-items: center;
  align-items: center;
}

#barr {
  width: 300px;
  height: 30px;
  background-color: #e6e6e6; /* Cambié el color de fondo */
  display: flex;
  align-items: center; /* Centrar verticalmente */
  border-radius: 15px; /* Redondear los bordes */
  overflow: hidden; /* Ocultar el contenido que desborda */
}

#barr #ball {
  height: 100%;
  width: 30px;
  background-color: #ff3333; /* Cambié el color del punto */
}

.flex-row {
  display: flex;
  flex-direction: row; /* Dirección normal */
}

.flex-row-reverse {
  display: flex;
  flex-direction: row-reverse; /* Dirección invertida */
}

#dice {
  font-size: 48px;
  font-weight: bold;
  color: #ffffff; /* Color blanco */
  background-color: #b0bec5; /* Color de fondo similar al color de un dado */
  width: 100px; /* Ancho */
  height: 100px; /* Altura */
  border-radius: 10px; /* Borde redondeado */
  display: flex;
  justify-content: center;
  align-items: center;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* Sombra de texto */
  margin: 10px auto;
}

.dice-roll {
  transform: scale(1.5); /* Escala para agrandar ligeramente */
}

</style>
