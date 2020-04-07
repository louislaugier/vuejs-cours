<template>
  <div id="board">
    <div class="header">
      <h1>Board 1</h1>
    </div>
    <div class="body">
      <NewListForm v-bind:onSubmit="onNewList" />
      <ul>
        <List v-for="list in lists" v-bind:key="list.id" v-bind="list" />
      </ul>
    </div>
  </div>
</template>

<script>
import List from "./List";
import NewListForm from "./NewListForm";
export default {
  name: "Board",
  components: {
    List,
    NewListForm
  },
  computed: {
    lists: function() {
      return this.getLists();
    }
  },
  inject: ["addList", "getLists"],
  methods: {
    onNewList: function(values) {
      //const data = new FormData(e.currentTarget);
      //console.log(Object.fromEntries(data.entries()));
      this.addList(values);
    }
  }
};
</script>