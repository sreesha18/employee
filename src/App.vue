<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const API_URL = 'https://69f9c002c509a40d3aa33e1e.mockapi.io/api/employees'


const employees = ref([])
const editId = ref(null)

const form = ref({
  name: '',
  designation: '',
  department: '',
  salary: ''
})


const getEmployees = async () => {
  const res = await axios.get(API_URL)
  employees.value = res.data
}


const addOrUpdateEmployee = async () => {
  if (editId.value) {
    await axios.put(`${API_URL}/${editId.value}`, form.value)
    editId.value = null
  } else {
    await axios.post(API_URL, form.value)
  }

  // Reset form
  form.value = {
    name: '',
    designation: '',
    department: '',
    salary: ''
  }

  getEmployees()
}

const editEmployee = (emp) => {
  form.value = { ...emp }
  editId.value = emp.id
}

const deleteEmployee = async (id) => {
  await axios.delete(`${API_URL}/${id}`)
  getEmployees()
}


onMounted(getEmployees)
</script>

<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4 text-primary">Employee Management System</h2>

    <!-- FORM CARD -->
    <div class="card p-4 shadow mb-4">
      <input v-model="form.name" class="form-control mb-2" placeholder="Name" />
      <input v-model="form.designation" class="form-control mb-2" placeholder="Designation" />
      <input v-model="form.department" class="form-control mb-2" placeholder="Department" />
      <input v-model="form.salary" class="form-control mb-2" placeholder="Salary" />

      <button @click="addOrUpdateEmployee" class="btn btn-success">
        {{ editId ? 'Update Employee' : 'Add Employee' }}
      </button>
    </div>

    <!-- TABLE CARD -->
    <div class="card p-3 shadow">
      <table class="table table-hover">
        <thead class="table-dark">
          <tr>
            <th>Name</th>
            <th>Designation</th>
            <th>Department</th>
            <th>Salary</th>
            <th>Actions</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="emp in employees" :key="emp.id">
            <td>{{ emp.name }}</td>
            <td>{{ emp.designation }}</td>
            <td>{{ emp.department }}</td>
            <td>{{ emp.salary }}</td>
            <td>
              <button @click="editEmployee(emp)" class="btn btn-warning btn-sm me-2">
                Edit
              </button>
              <button @click="deleteEmployee(emp.id)" class="btn btn-danger btn-sm">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>