<template>
  <div class="game container">
    <div class="columns">
      <div class="column is-6">
        <h2>Player:</h2>
        <div class="game__live-bar-container">
          <div class="game__live-bar"></div>
        </div>
      </div>
      <div class="column is-6">
        <h2>Enemy:</h2>
        <div class="game__live-bar-container">
          <div class="game__live-bar"></div>
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
        <button class="button is-warning">Sword</button>
        <button class="button is-danger">Axe</button>
        <button class="button is-info">Heal</button>
        <button @click.prevent="gameRuningHandler" class="button is-dark">Give up</button>
      </div>
    </div>
    <div class="columns"></div>
  </div>
</template>

<script>
export default {
  name: "Main",
  data() {
    return {
      gameIsRunning: false
    };
  },
  methods: {
    gameRuningHandler() {
      this.gameIsRunning = !this.gameIsRunning;
    }
  }
};
</script>

<style lang="sass" scoped >
  @import "bulma/css/bulma.css"

  .game
    height: calc(100vh - 102px)
    padding: 50px 0
</style>
