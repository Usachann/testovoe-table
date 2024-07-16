<template>
  <DefaultModal @close="close">
    <template v-slot:header>
      <p>Добавить сотрудника</p>
    </template>

    <template v-slot:body>
      <form>
        <DefaultTextInput
          label="Имя"
          placeholder="Иван"
          type="text"
          v-model="name"
        />
        <span v-if="nameError" class="error-text">Имя обязательно</span>

        <DefaultTextInput
          label="Телефон"
          placeholder="7 XXX XX XX XXX"
          type="tel"
          v-model="phone"
          :maxlength="12"
        />
        <span v-if="phoneError" class="error-text"
          >Некорректный формат телефона</span
        >

        <div class="parent-select-container">
          <label>
            Начальник

            <select v-model="parentId" class="parent-select">
              <option disabled value="null">Выберите начальника</option>
              <option value="">Главный начальник</option>

              <option
                v-for="employee in allEmployees"
                :key="employee.id"
                :value="employee.id"
              >
                {{ employee.name }}
              </option>
            </select>
          </label>
        </div>
      </form>
    </template>

    <template v-slot:footer>
      <DefaultButton @click="submit"> Сохранить </DefaultButton>

      <p class="error-text" v-if="formError">Заполните все поля *</p>
    </template>
  </DefaultModal>
</template>

<script>
import DefaultButton from "./UI/DefaultButton.vue";
import DefaultModal from "./UI/DefaultModal.vue";
import DefaultTextInput from "./UI/DefaultTextInput.vue";

import { uuid } from "vue-uuid";
import { flattenArray } from "@/utils/arrayUtils";

export default {
  name: "EmployeeFormModal",
  components: {
    DefaultModal,
    DefaultButton,
    DefaultTextInput,
  },
  props: {
    employees: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      name: null,
      phone: null,
      parentId: null,
      phoneError: false,
      formError: false,
      nameError: false,
    };
  },
  emits: {
    close: null,
    submit: null,
  },
  computed: {
    allEmployees() {
      return flattenArray(this.employees);
    },
    isFormFilled() {
      return (
        Boolean(this.name) && Boolean(this.phone) && this.parentId !== null
      );
    },
  },
  methods: {
    formatPhone(phone) {
      const cleaned = ("" + phone).replace(/\D/g, "");
      const match = cleaned.match(/^(\d{1})(\d{3})(\d{3})(\d{2})(\d{2})$/);
      if (match) {
        return `+${match[1]}(${match[2]})${match[3]}-${match[4]}-${match[5]}`;
      }
      return null;
    },
    validatePhone(phone) {
      const phoneRegExp = /^\+7\(\d{3}\)\d{3}-\d{2}-\d{2}$/;
      return phoneRegExp.test(phone);
    },
    submit() {
      this.phone = this.formatPhone(this.phone);
      this.phoneError = !this.validatePhone(this.phone);
      this.nameError = !this.name;
      this.formError = !this.isFormFilled;

      if (this.nameError) {
        return;
      }

      if (this.phoneError) {
        return;
      }

      if (!this.formError) {
        const newEmployeeLevel = this.parentId
          ? this.allEmployees.find((employee) => employee.id === this.parentId)
              .level + 1
          : 0;

        this.$emit("submit", {
          id: uuid.v1(),
          name: this.name,
          phone: this.phone,
          parentId: this.parentId ? this.parentId : null,
          level: newEmployeeLevel,
          isChildrenOpen: false,
        });
        this.close();
      }
    },
    close() {
      this.$emit("close");
    },
  },
};
</script>
<style scoped>
.parent-select-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-direction: column;
  margin-top: 1rem;
}

.parent-select {
  width: 100%;
  border: 0;
  padding: 0.5rem;
}

.error-text {
  color: var(--error-text-color);
}
</style>
