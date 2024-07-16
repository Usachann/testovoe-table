<template>
  <div @click="close" class="modal-backdrop">
    <div @click.stop class="modal">
      <div class="header">
        <slot name="header"></slot>
      </div>

      <div class="body">
        <slot name="body"></slot>
      </div>

      <div class="footer">
        <slot name="footer"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DefaultModal",
  emits: {
    close: null,
  },
  methods: {
    close() {
      this.$emit("close")
    },
  },
}
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background: var(--bg-color);
  overflow-x: auto;
  display: flex;
  flex-direction: column;
  border-radius: 10px;
  padding: 2rem;
}

.header {
  font: var(--font-l);
}

.body {
  margin-top: 0.5em;
}

.footer {
  margin-top: 2em;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
</style>
