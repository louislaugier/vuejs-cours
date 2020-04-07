<template>
  <form v-on:submit.prevent="handleSubmit" id="new-card">
    <a @click="toggleModal">Add new card</a>
    <Modal :open="open" :onCancel="toggleModal">
      <template slot="title">
        <h2>New Card</h2>
      </template>

      <input v-model="title" type="text" />
      <input v-model="status" type="checkbox" />

      <template slot="actions">
        <input class="modal-action" type="submit" value="Add Card" />
      </template>
    </Modal>
  </form>
</template>

<script>
import Modal from "./Modal";
export default {
  name: "ModalNewCardForm",
  components: {
    Modal
  },
  data: () => ({
    title: "",
    status: false,
    open: false
  }),
  props: {
    onSubmit: Function
  },
  methods: {
    handleSubmit: function() {
      this.toggleModal();
      this.onSubmit({ ...this.$data });
    },
    toggleModal: function() {
      this.open = !this.open;
    }
  }
};
</script>