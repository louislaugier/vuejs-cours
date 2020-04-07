<template>
  <div id="app">
    <BoardProvider :id="1">
      <Board />
    </BoardProvider>
  </div>
</template>

<script>
import Board from "./components/Board.vue";
import BoardProvider from "./BoardProvider.vue";
export default {
  name: "app",
  components: {
    BoardProvider,
    Board
  }
};
</script>

<style scoped>
div {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>