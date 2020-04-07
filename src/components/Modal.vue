<template>
  <div v-if="open" class="modal">
    <div class="modal-title">
      <slot name="title">
        <h1>Modal</h1>
      </slot>
      <a class="modal-close" @click.prevent="onCancel">X</a>
    </div>
    <div class="modal-body">
      <slot name="default" />
    </div>
    <div class="modal-actions">
      <slot name="actions">
        <a class="modal-action" @click.prevent="onCancel">Cancel</a>
        <a class="modal-action" @click.prevent="onAccept">Accept</a>
      </slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  props: {
    open: Boolean,
    onCancel: Function,
    onAccept: Function
  }
};
</script>

<style scoped>
.modal {
  position: fixed;
  margin: 0 auto;
  top: 50%;
  width: 75%;
  height: 50%;
  transform: translateY(-50%);
  background-color: pink;
  display: flex;
  flex-direction: column;
}
.modal-title {
  display: flex;
  width: 100%;
}
.modal-title h1 {
  flex: 1;
}
.modal-close {
  margin-right: 10;
}
.modal-body {
  flex: 1;
}
.modal-actions {
  display: flex;
  justify-content: space-between;
}
.modal-actions .modal-action {
  background-color: red;
}
</style>