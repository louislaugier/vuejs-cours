<template>
  <form v-on:submit.prevent="handleSubmit" id="new-list">
    <input v-model="name" type="text" v-on:keyup.v="onKeyV" />
    <input type="submit" value="Add" />
  </form>
</template>

<script>
export default {
  name: "NewListForm",
  data: () => ({
    name: ""
  }),
  props: {
    onSubmit: Function
  },
  methods: {
    onKeyV: () => console.log("V pressed"),
    handleSubmit: function() {
      this.onSubmit(this.$data);
    }
  }
};
</script>
