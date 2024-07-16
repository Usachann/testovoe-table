<template>
  <div class="employee-management-container">
    <DefaultButton @click="openEmployeeFormModal"> Добавить </DefaultButton>

    <EmployeeTable
      v-if="tableHasEmployees"
      @sortEmployees="sortEmployees"
      @updateEmployees="updateEmployees"
      :employees="employees"
    />
    <p v-else>Нет добавленных сотрудников</p>
  </div>

  <EmployeeFormModal
    v-if="isModalVisible"
    @close="closeEmployeeFormModal"
    @submit="addNewEmployee"
    :employees="employees"
  />
</template>

<script>
import DefaultButton from "@/components/UI/DefaultButton.vue";
import EmployeeTable from "@/components/EmployeeTable/App.vue";
import EmployeeFormModal from "@/components/EmployeeFormModal.vue";
import { addItemToArray, sortArray } from "@/utils/arrayUtils";

export default {
  name: "App",
  components: {
    DefaultButton,
    EmployeeTable,
    EmployeeFormModal,
  },
  data() {
    return {
      isModalVisible: false,
      employees: null,
    };
  },
  created() {
    this.loadEmployeesFromLocalStorage();
  },
  computed: {
    tableHasEmployees() {
      return this.employees.length > 0;
    },
  },
  methods: {
    loadEmployeesFromLocalStorage() {
      const employees = JSON.parse(localStorage.getItem("employees")) || [];
      this.employees = employees;
    },
    addNewEmployee(newEmployee) {
      addItemToArray(this.employees, newEmployee);
      this.addEmployeesToLocalStorage();
    },
    sortEmployees() {
      this.employees = sortArray(this.employees);
      this.addEmployeesToLocalStorage();
    },
    openEmployeeFormModal() {
      this.isModalVisible = true;
    },
    closeEmployeeFormModal() {
      this.isModalVisible = false;
    },
    addEmployeesToLocalStorage() {
      localStorage.setItem("employees", JSON.stringify(this.employees));
    },
    updateEmployees(newEmployees) {
      this.employees = newEmployees;
      this.addEmployeesToLocalStorage();
    },
  },
};
</script>

<style scoped>
.employee-management-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 100%;
  gap: 1em;
  padding: 1em;
}
</style>
