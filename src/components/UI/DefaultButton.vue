<template>
  <button type="button" class="main-button">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: "DefaultButton",
};
</script>

<style scoped>
.main-button {
  all: unset;
  display: inline-block;
  padding: 0.5rem 2rem;
  cursor: pointer;
  background-color: #007bff;
  color: #fff;
  text-decoration: none;
  border-radius: 20px;
  transition: background-color 0.3s;
}

.favorite-button:hover {
  background-color: #0056b3;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
}

.main-button:active {
  transform: scale(0.99);
}
</style>
