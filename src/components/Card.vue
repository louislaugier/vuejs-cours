  
<template>
  <div v-on:click="handleClick" id="card">
    <div class="header">
      <h1>{{ title }}</h1>
    </div>
    <div class="body">
      <span v-if="status === true">Done</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    id: Number,
    title: String,
    status: Boolean
  },
  methods: {
    handleClick: function() {
      this.$emit("toggle-card", { id: this.id });
    }
  }
};
</script>

<style scoped>
h1 {
  font-size: 1em;
}
</style>