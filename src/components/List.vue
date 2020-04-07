<template>
  <div id="list">
    <div class="header">
      <h1>
        {{ name }}
        <span v-if="cardCount > 0">{{ cardCount }}</span>
        <span v-show="cardCount > 0">{{ cardCount }}</span>
      </h1>
    </div>
    <div class="body">
      <ul>
        <li v-for="card in cards" v-bind:key="card.id">{{ card.name }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "List",
  props: {
    id: Number,
    name: String,
    cards: {
      type: Array,
      default: () => []
    }
  },
  computed: {
    cardCount: function() {
      return this.cards.length;
    }
  },
  inject: ["getList"]
};
</script>