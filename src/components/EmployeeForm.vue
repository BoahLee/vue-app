<template>
  <div id="employee-form">
    <form v-on:submit.prevent="handleSubmit">
      <label for="">Employee Name</label>
      <input
        type="text"
        v-model="employee.name"
        v-bind:class="{ 'has-error': submitted && invalidName }"
        v-on:focus="clearStatus"
        v-on:keypress="clearStatus"
        ref="first"
      />
      <label for="">Employee Email</label>
      <input
        type="text"
        v-model="employee.email"
        v-bind:class="{ 'has-error': submitted && invalidEmail }"
        v-on:focus="clearStatus"
      />

      <p v-if="error && submitted" class="error-message">
        ❗ Please fill out all required fields
      </p>
      <p v-if="success" class="success-message">
        ✅ Employee successfully added
      </p>
      <button class="add-button">Add Employee</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "employee-form",
  data() {
    return {
      submitted: false,
      error: false,
      success: false,
      employee: {
        name: "",
        email: "",
      },
    };
  },
  methods: {
    handleSubmit() {
      this.submitted = true;
      this.clearStatus();

      if (this.invalidName || this.invalidEmail) {
        this.error = true;
        return;
      }

      this.$emit("add:employee", this.employee);
      this.$refs.first.focus();
      this.employee = {
        name: "",
        email: "",
      };

      this.error = false;
      this.success = true;
      this.submitted = false;
    },

    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },
  computed: {
    invalidName() {
      return this.employee.name === "";
    },
    invalidEmail() {
      return this.employee.email === "";
    },
  },
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}

.add-button {
  background-color: #0250bc;
  border: none;
}

.add-button:hover {
  background-color: #1034a6;
}
</style>
