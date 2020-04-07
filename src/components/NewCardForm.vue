<template>
  <form v-on:submit.prevent="handleSubmit" id="new-card">
    <input v-model="title" type="text" />
    <input v-model="status" type="checkbox" />
    <input type="submit" value="Add Card" />
  </form>
</template>

<script>
export default {
  name: "NewCardForm",
  data: () => ({
    title: "",
    status: false
  }),
  props: {
    onSubmit: Function
  },
  methods: {
    handleSubmit: function() {
      this.onSubmit({ ...this.$data });
    }
  }
};
</script>