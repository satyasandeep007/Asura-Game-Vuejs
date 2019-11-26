 <template>
  <div id="game">
    <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">ASURA</h1>
            <div class="healthbar">
                <div
                        class="healthbar text-center"
                        style="background-color: green; margin: 0; color: white;"
                        :style="{width: playerHealth + '%'}">
                    {{ playerHealth }}
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">COMPUTER</h1>
            <div class="healthbar">
                <div
                        class="healthbar text-center"
                        style="background-color: green; margin: 0; color: white;"
                        :style="{width: monsterHealth + '%'}">
                    {{ monsterHealth }}
                </div>
            </div>
        </div>
    </section>

    <section class="row controls" v-if="!gameIsRunning">
      <div class="small-12 columns">
        <button
          id="start-game"
          @click="startGame"
          type="button"
          class="btn btn-success"
        >START NEW GAME</button>
      </div>
    </section>
    <section class="row controls" v-else>
      <div class="small-12 columns">
        <button id="attack" @click="attack" type="button" class="btn btn-primary">ATTACK</button>
        <button
          id="special-attack"
          @click="specialAttack"
          type="button"
          class="btn btn-info"
        >SPECIAL ATTACK</button>
        <button id="heal" @click="heal" type="button" class="btn btn-warning">HEAL</button>
        <button id="give-up" @click="giveUp" type="button" class="btn btn-danger">GIVE UP</button>
      </div>
    </section>
  </div>
</template>
 
  <script>
export default {
  name: "#game",
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      gameIsRunning: false
    };
  },
  methods: {
    startGame: function() {
      this.gameIsRunning = true;
      this.playerHealth = 100;
      this.monsterHealth = 100;
    },
    attack: function() {
      let damage = this.calculateDamage(3, 10);
      this.monsterHealth -= damage;
      if (this.checkWin()) {
        return;
      }
      this.monsterAttacks();
    },
    specialAttack: function() {
      let damage = this.calculateDamage(10, 20);
      this.monsterHealth -= damage;
      if (this.checkWin()) {
        return;
      }
      this.monsterAttacks();
    },
    heal: function() {
      if (this.playerHealth <= 90) {
        this.playerHealth += 10;
      } else {
        this.playerHealth = 100;
      }
      this.monsterAttacks();
    },
    giveUp: function() {
      this.gameIsRunning = false;
      alert("Thanks for playing! Please play again");
    },
    monsterAttacks: function() {
      let damage = this.calculateDamage(5, 12);
      this.playerHealth -= damage;
      this.checkWin();
    },
    calculateDamage: function(min, max) {
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    checkWin: function() {
      if (this.monsterHealth <= 0) {
        if (confirm("You won! Wanna play again?")) {
          this.startGame();
        } else {
          this.gameIsRunning = false;
        }
        return true;
      } else if (this.playerHealth <= 0) {
        if (confirm("You lost! Wanna try again?")) {
          this.startGame();
        } else {
          this.gameIsRunning = false;
        }
        return true;
      }
      return false;
    }
  }
};
</script>
 
<style scoped>
* {
  margin: 3% auto;
}.text-center {
    text-align: center;
}

.healthbar {
    width: 100%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    padding: 12px;
    margin: 10px;
}
</style> 