<template>
  <div class="game container">
    <div class="columns">
      <div class="column is-6">
        <h2>Player:</h2>
        <div class="game__live-bar-container">
          <div
            class="game__live-bar game__live-bar--player has-text-centered"
            :style="{width: playerEnegry + '%'}"
          >{{ playerEnegry }}</div>
        </div>
      </div>
      <div class="column is-6">
        <h2>Enemy:</h2>
        <div class="game__live-bar-container">
          <div
            class="game__live-bar game__live-bar--enemy has-text-centered"
            :style="{width: enemyEnergy + '%'}"
          >{{ enemyEnergy }}</div>
        </div>
      </div>
    </div>
    <div v-if="!gameIsRunning" class="columns is-centered">
      <div class="column has-text-centered">
        <button @click.prevent="gameRuningHandler" class="button is-info">Start</button>
      </div>
    </div>
    <div v-else class="columns is-centered">
      <div class="column is-5 has-text-centered">
        <button @click.prevent="playerAttack" class="button is-warning" id="sword">Sword</button>
        <button @click.prevent="playerAttack" class="button is-danger" id="axe">Axe</button>
        <button @click.prevent="playerHeal" class="button is-info">Heal</button>
        <button @click.prevent="gameRuningHandler" class="button is-dark">Give up</button>
      </div>
    </div>
    <div class="columns">
      <div class="column is-12 has-text-centered">
        <h2 class="is-size-5">Action history:</h2>
      </div>
    </div>
    <div class="columns">
      <div class="column">
        <div v-for="(action, index) in actions" class="game__log has-text-centered" :key="index">
          <span class="game__character">{{ action.character }}</span>
          <span class="game__attack">{{ action.attack }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      gameIsRunning: false,
      playerEnegry: 100,
      enemyEnergy: 100,
      result: "",
      actions: []
    };
  },
  methods: {
    gameRuningHandler() {
      this.gameIsRunning = !this.gameIsRunning;
      this.playerEnegry = 100;
      this.enemyEnergy = 100;
      this.actions = [];
    },
    playerAttack(e) {
      const weapon = e.target.id;
      let damage;

      if (weapon === "sword") {
        damage = Math.floor(Math.random() * (8 - 4) + 4);
        this.takeEnemyEnergy(damage);
      } else if (weapon === "axe") {
        damage = Math.floor(Math.random() * (12 - 1) + 1);
        this.takeEnemyEnergy(damage);
      }
    },
    playerHeal() {
      const heal = Math.floor(Math.random() * (10 - 5) + 5);
      this.playerEnegry = this.playerEnegry + heal;

      if (this.playerEnegry > 100) {
        this.playerEnegry = 100;
      }

      this.actions.unshift({
        character: "Player: ",
        attack: `heal by ${heal}`
      });

      this.enemyAttack();
    },
    enemyAttack() {
      let damage = Math.floor(Math.random() * (10 - 4) + 4);
      this.takePlayerEnergy(damage);
    },
    takeEnemyEnergy(damage) {
      this.enemyEnergy = this.enemyEnergy - damage;
      this.actions.unshift({
        character: "Player: ",
        attack: `attack by ${damage}`
      });

      if (this.enemyEnergy <= 0) {
        this.enemyEnergy = 0;
        this.result = "You won!";
        this.displayResult();
      } else {
        this.enemyAttack();
      }
    },
    takePlayerEnergy(damage) {
      this.playerEnegry = this.playerEnegry - damage;
      this.actions.unshift({
        character: "Enemy: ",
        attack: `attack by ${damage}`
      });

      if (this.playerEnegry <= 0) {
        this.playerEnegry = 0;
        this.result = "You Lose!";
        this.displayResult();
      }
    },
    displayResult() {
      alert(this.result);
      this.gameIsRunning = false;
      this.playerEnegry = 100;
      this.enemyEnergy = 100;
      this.actions = [];
    }
  }
};
</script>

<style lang="sass" scoped >
  @import "bulma/css/bulma.css"

  .game
    min-height: calc(100vh - 102px)
    padding: 50px 20px
    &__live-bar-container
      border: 1px solid #DDDDDD
      margin-top: 10px
      display: flex
    &__live-bar
      padding: 10px
      display: flex
      transition: width .1s ease
      &--player
        background-color: #00D1B2
      &--enemy
        background-color: #FF3860
    &__log
      padding: 5px 0
    &__character
      font-weight: bold 
</style>
