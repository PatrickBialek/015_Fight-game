<template>
  <div class="game container"></div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  }
};
</script>

<style lang="sass" scoped >
  @import "bulma/css/bulma.css"

  .game
    height: calc(100vh - 102px)
</style>
