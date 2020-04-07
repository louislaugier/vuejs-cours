<template>
  <div id="app">
    <Board title="Board 1" :lists="lists" />
  </div>
</template>

<script>
import Board from "./components/Board.vue";

export default {
  name: "app",
  components: {
    Board
  },
  data: () => {
    return {
      lists: [
        {
          id: 1,
          name: "Todo",
          cards: [
            { id: 1, name: "Card1" },
            { id: 2, name: "Card2" }
          ]
        },
        { id: 2, name: "Doing" },
        { id: 3, name: "Done" }
      ]
    };
  },
  methods: {
    addList: function(list) {
      this.lists = [...this.lists, { id: Date.now(), ...list }];
    },
    getList: function(id) {
      return this.lists.find(list => list.id === id);
    }
  },
  provide: function() {
    return {
      addList: this.addList,
      getList: this.getList
    };
  }
};
</script>

<style scoped>
div {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
