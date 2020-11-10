<template>
  <div id="employee-table">
    <p v-if="employees.length < 1" class="empty-table">No employees found</p>
    <table v-else>
      <thead>
        <tr>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="employee in employees" v-bind:key="employee.id">
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.name" />
          </td>
          <td v-else>{{ employee.name }}</td>
          <td v-if="editing === employee.id">
            <input type="text" v-model="employee.email" />
          </td>
          <td v-else>{{ employee.email }}</td>
          <td v-if="editing === employee.id">
            <button @click="editEmployee(employee)">Save</button>
            <button @click="cancelEdit(employee)" class="muted-button">
              Cancel
            </button>
          </td>
          <td v-else>
            <button class="edit-button" @click="editMode(employee)">
              Edit
            </button>
            <button
              class="delete-button"
              @click="$emit('delete:employee', employee.id)"
            >
              Delete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "employee-table",
  props: {
    employees: Array,
    // or props: ['employees']
  },
  data() {
    return {
      editing: null,
    };
  },
  methods: {
    editMode(employee) {
      this.cachedEmployee = { ...employee };
      this.editing = employee.id;
    },

    cancelEdit(employee) {
      // employee = { ...this.cachedEmployee };
      // console.log("111", employee);
      Object.assign(employee, this.cachedEmployee);
      // console.log("222", employee);
      this.editing = null;
    },

    editEmployee(employee) {
      if (employee.name === "" || employee.email === "") return;
      this.$emit("edit:employee", employee.id, employee);
      this.editing = null;
    },
  },
};
</script>

<style scoped>
button {
  margin: 0 0.5rem 0 0;
}

.edit-button:hover {
  background-color: #026440;
}

.delete-button {
  background-color: #f03949;
  border: none;
}

.delete-button:hover {
  background-color: #d11a2a;
}
</style>
