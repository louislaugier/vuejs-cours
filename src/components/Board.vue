<template>
  <div id="board">
    <div class="header">
      <h1>{{ title }}</h1>
    </div>
    <div class="body">
      <NewListForm v-bind:onSubmit="onNewList" />
      <ul>
        <button v-on:click="addList">Add List</button>
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
  props: {
    title: String,
    lists: Array
  },
  inject: ["addList"],
  methods: {
    onNewList: function(values) {
      //const data = new FormData(e.currentTarget);
      //console.log(Object.fromEntries(data.entries()));
      this.addList(values);
    }
  }
};
</script>
