<template>
  <div>
    <slot name="default" />
  </div>
</template>

<script>
export default {
  name: "BoardProvider",
  props: {
    id: Number
  },
  data: () => {
    return {
      lists: []
    };
  },
  mounted: function() {
    fetch("http://localhost:3004/boards/" + this.id + "/lists")
      .then(response => response.json())
      .then(data => (this.lists = data));
  },
  methods: {
    addList: function(list) {
      this.lists = [...this.lists, { id: Date.now(), ...list }];
    },
    getList: function(id) {
      return this.lists.find(list => list.id === id);
    },
    getCard: function(listId, cardId) {
      return this.lists
        .find(list => list.id === listId)
        .cards.find(card => card.id == cardId);
    },
    getLists: function() {
      return this.lists;
    },
    addCardToList: function(listId, _card) {
      fetch("http://localhost:3004/cards", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          listId,
          ..._card
        })
      })
        .then(response => response.json())
        .then(card => {
          this.lists = this.lists.map(list => {
            if (list.id === listId) {
              list = {
                ...list,
                cards: [...(list.cards || []), card]
              };
            }
            return list;
          });
        });
    },
    toggleCard: function(listId, cardId) {
      const _card = this.getCard(listId, cardId);
      _card.status = !_card.status;
      fetch("http://localhost:3004/cards/" + _card.id, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          listId,
          ..._card
        })
      })
        .then(response => response.json())
        .then(fcard => {
          const list = this.getList(listId);
          list.cards = list.cards.map(card => {
            if (card.id === fcard.id) {
              card = fcard;
            }
            return card;
          });
        });
    },
    fetchCards: function(listId) {
      fetch("http://localhost:3004/lists/" + listId + "/cards")
        .then(response => response.json())
        .then(data => {
          this.lists = this.lists.map(list => {
            if (list.id === listId) {
              list = {
                ...list,
                cards: data
              };
            }
            return list;
          });
        });
    }
  },
  provide: function() {
    return {
      addList: this.addList,
      getList: this.getList,
      getLists: this.getLists,
      addCardToList: this.addCardToList,
      toggleCard: this.toggleCard,
      fetchCards: this.fetchCards
    };
  }
};
</script>