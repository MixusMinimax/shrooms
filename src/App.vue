<template>
  <h1>Hello World!</h1>
  <Game
    :width="7"
    :height="7"
  />
</template>

<script>
import Game from './components/Game.vue'

export default {
  name: 'App',
  components: {
    Game,
  },
}
</script>

<style>

</style>
