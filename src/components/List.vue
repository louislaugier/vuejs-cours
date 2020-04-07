<template>
  <div id="list">
    <div class="header">
      <h1>
        {{ title }}
        <span v-if="cardCount > 0">{{ cardNotDoneCount }}/{{ cardCount }}</span>
      </h1>
    </div>
    <div class="body">
      <NewCardForm :onSubmit="addCard" />
      <ul>
        <Card
          v-for="card in cards"
          v-bind:key="card.id"
          v-bind="card"
          v-on:toggle-card="onToggleCard"
        />
      </ul>
    </div>
  </div>
</template>

<script>
import Card from "./Card";
import NewCardForm from "./ModalNewCardForm";
export default {
  name: "List",
  components: {
    Card,
    NewCardForm
  },
  props: {
    id: Number,
    title: String,
    cards: {
      type: Array,
      default: () => []
    }
  },
  mounted: function() {
    this.fetchCards(this.id);
  },
  computed: {
    cardCount: function() {
      return this.cards.length;
    },
    cardNotDoneCount: function() {
      return this.cards.filter(card => !card.status).length;
    }
  },
  inject: ["getList", "addCardToList", "toggleCard", "fetchCards"],
  methods: {
    addCard: function(card) {
      this.addCardToList(this.id, card);
    },
    onToggleCard: function(e) {
      this.toggleCard(this.id, e.id);
    }
  }
};
</script>