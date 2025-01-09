<template>
  <div>
    <div id="game" class="z-1 relative">
      <GameComponents />
    </div>
  </div>
</template>

<script>
import GameComponents from "./components/Game.vue";
export default {
  components: { GameComponents },
  name: "game",
};
</script>

<style>
/* Your styles here */
</style>
