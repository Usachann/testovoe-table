<template>
  <table class="table">
    <thead>
      <tr @click="handleTableHeadClick" class="header">
        <th class="head-cell">Имя</th>

        <th class="head-cell">Телефон</th>
      </tr>
    </thead>

    <tbody>
      <table-body @updateEmployees="updateEmployees" :employees="employees" />
    </tbody>
  </table>
</template>

<script>
import TableBody from "./TableBody.vue"

export default {
  name: "EmployeeTable",
  components: {
    TableBody,
  },
  props: {
    employees: {
      type: Array,
      default: () => [],
    },
  },
  emits: {
    sortEmployees: null,
    updateEmployees: null,
  },
  methods: {
    handleTableHeadClick() {
      this.$emit("sortEmployees")
    },
    updateEmployees(newEmployees) {
      this.$emit("updateEmployees", newEmployees)
    },
  },
}
</script>

<style scoped>
.table {
  width: 100%;
  border-collapse: collapse;
}

.header {
  cursor: pointer;
  background: var(--bg-secondary-color);
}
.head-cell {
  font: var(--font-l);
  padding: 0.5rem;
  width: 50%;
  border: 1px solid var(--bg-secondary-color);
  text-align: left;
  padding-left: 1rem;
  color: #ffffff;
}
</style>
