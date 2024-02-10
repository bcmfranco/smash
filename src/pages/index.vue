<template>
  <div id="container">
    <h1 id="logo">SMASH</h1>

    <div id="pointer">
      <div class="player-score">{{ points.player_1 }}</div>
      <div class="player-score">{{ points.player_2 }}</div>
    </div>

    <div id="court" :style="{ backgroundColor: courtBackgroundColor }">
      <div id="mensenger">{{ this.fading_2 }}</div>
      <div id="barr" :class="{ 'flex-row': player_active === 1, 'flex-row-reverse': player_active === 2 }">
        <div id="ball"></div>
      </div>
    </div>

    <div id="joystick">
      <div id="dice">
        <div :class="{ 'dice-roll': isRolling }" v-if="!diceRolling">{{ diceValue }}</div>
      </div>

      <div id="controlers">
        <button class="racket" id="racket_p1" @click="golpeP1">P1</button>
        <button class="racket" id="racket_p2" @click="golpeP2">P2</button>
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
      fadding_2: null,
      showElement: true,
      diceValue: 1,
      diceRolling: false,
      courtBackgroundColor: '#f2f2f2', // Color de fondo inicial del #court
    }
  },
  methods: {
    rollDice() {
      this.diceRolling = true;
      setTimeout(() => {
        this.diceValue = Math.floor(Math.random() * 6) + 1;
        this.diceRolling = false;
      }, 1000);
      return this.diceValue;
    },
    golpeP1() {
      if (this.last_player_golpe !== 1) {
        this.last_player_golpe = 1;
        this.player_active = 2;
        this.diceValue = this.rollDice();
        this.fading_msg = this.energy;
        this.fading_2 = this.energy;
        if (this.diceValue - this.energy > 0) {
          this.energy = this.diceValue - this.energy;
          this.fading_msg += this.diceValue + " >>> " + this.energy;
          this.fading_2 = this.energy;
        } else {
          this.fading_msg += this.diceValue + " FAIL";
          this.fading_2 = "Fail";
          this.missPoint();
          this.wonPoint(2);
          this.courtBackgroundColor = '#ff6666'; // Cambiar color de fondo en caso de 'Fail'
          setTimeout(() => {
            this.courtBackgroundColor = '#f2f2f2'; // Restaurar color de fondo después de 1 segundo
          }, 1000);
        }
      }
    },
    golpeP2() {
      this.last_player_golpe = 2;
      this.player_active = 1;
      this.diceValue = this.rollDice();
      this.fading_msg = this.energy + " >>> ";
      this.fading_2 = this.energy;
      if (this.diceValue - this.energy > 0) {
        this.energy = this.diceValue - this.energy;
        this.fading_msg += this.diceValue + " >>> " + this.energy;
        this.fading_2 = this.energy;
      } else {
        this.fading_msg += this.diceValue + " FAIL";
        this.fading_2 = "FAIL";
        this.missPoint();
        this.missPoint();
        this.wonPoint(1);
        this.courtBackgroundColor = '#ff6666'; // Cambiar color de fondo en caso de 'Fail'
        setTimeout(() => {
          this.courtBackgroundColor = '#f2f2f2'; // Restaurar color de fondo después de 1 segundo
        }, 1000);
      }
    },
    missPoint() {
      this.energy = 0;
      console.log("!", this.energy);
    },
    wonPoint(player) {
      if (player === 1) {
        this.points.player_1++; // Sumar un punto al jugador 1
        if (this.points.player_1 > 6) {
          this.wonSet(1);
        }
      } else if (player === 2) {
        this.points.player_2++; // Sumar un punto al jugador 2
        if (this.points.player_2 > 6) {
          this.wonSet(2);
        }
      }
    },
    wonSet(player) {
      console.log(player, "won the set");
    },
    restartMatch() {
      // Reinicia el puntaje y el estado del juego
      this.points.player_1 = 0;
      this.points.player_2 = 0;
      this.energy = 0;
      this.last_player_golpe = null;
      this.player_active = 1;
      this.fading_msg = null;
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
  background-color: #f2f2f2;
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
  color: #ffffff;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  margin-bottom: 20px;
}

#joystick {
  width: 300px;
  height: 200px;
  border: 1px solid #cccccc;
  background-color: #ffffff;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.points {
  font-size: 24px;
  border: none;
  background-color: #e6e6e6;
  color: #333333;
  border-radius: 10px;
  text-align: center;
  display: grid;
  align-items: center;
}

#controlers {
  display: grid;
  gap: 10px;
  grid-template-columns: repeat(2, 1fr);
  justify-items: center;
  align-items: center;
}

.racket {
  width: 80px;
  height: 80px;
  font-size: 24px;
  border: none;
  background-color: #66cc66;
  color: #ffffff;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.racket:hover {
  background-color: #4caf50;
}

#buttoner {
  margin-top: 20px;
}

#restart_btn {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #ff6666;
  color: #ffffff;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

#restart_btn:hover {
  background-color: #ff3333;
}

#court {
  border: 2px solid #999999;
  width: 400px;
  height: 150px;
  margin: 10px auto 20px auto;
  border-radius: 15px;
  display: grid;
  justify-items: center;
  align-items: center;
  transition: background-color 1s ease; /* Transición de color de fondo */
}

#barr {
  width: 300px;
  height: 30px;
  background-color: #e6e6e6;
  display: flex;
  align-items: center;
  border-radius: 15px;
  overflow: hidden;
}

#barr #ball {
  height: 100%;
  width: 30px;
  background-color: #ff3333;
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
  background-color: #b0bec5;
  width: 80px;
  height: 80px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  margin: 10px auto;
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
  border: 2px solid #000;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  font-weight: bold;
  margin: 0 10px;
  background-color: #fff;
}

</style>
