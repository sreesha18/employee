<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const API_URL = 'https://69f9cf08c509a40d3aa35b2b.mockapi.io/api/employee'

const name = ref('')
const designation = ref('')
const employeeid = ref('')
const salary = ref('')

const employees = ref([])

// Fetch employees
const getEmployees = async () => {
  const res = await axios.get(API_URL)
  employees.value = res.data
}

// Add employee
const addEmployee = async () => {
  if (!name.value || !designation.value || !employeeid.value || !salary.value) {
    alert('Fill all fields')
    return
  }

  await axios.post(API_URL, {
    name: name.value,
    designation: designation.value,
    employeeid: employeeid.value,
    salary: Number(salary.value)
  })

  // Clear inputs
  name.value = ''
  designation.value = ''
  employeeid.value = ''
  salary.value = ''

  getEmployees()
}

// Delete employee
const deleteEmployee = async (id) => {
  await axios.delete(`${API_URL}/${id}`)
  getEmployees()
}

onMounted(getEmployees)
</script>

<template>
  <div class="container">
    <h1>Employee Management</h1>

    <div class="form">
      <input v-model="name" placeholder="Name" />
      <input v-model="designation" placeholder="Designation" />
      <input v-model="employeeid" placeholder="Employee ID" />
      <input v-model="salary" placeholder="Salary" type="number" />

      <button @click="addEmployee">Add Employee</button>
    </div>

    <div class="list">
      <div v-for="emp in employees" :key="emp.id" class="card">
        <p><b>Name:</b> {{ emp.name }}</p>
        <p><b>Designation:</b> {{ emp.designation }}</p>
        <p><b>ID:</b> {{ emp.employeeid }}</p>
        <p><b>Salary:</b> ₹{{ emp.salary }}</p>

        <button @click="deleteEmployee(emp.id)">Delete</button>
      </div>
    </div>
  </div>
</template>

<style>
.container {
  text-align: center;
  padding: 20px;
  background: #ffe4ec;
  min-height: 100vh;
}

h1 {
  color: #d63384;
}

.form input {
  display: block;
  margin: 10px auto;
  padding: 10px;
  width: 250px;
  border: 2px solid #ff4d6d;
  border-radius: 8px;
}

button {
  padding: 10px 20px;
  margin: 10px;
  background: #ff4d6d;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button:hover {
  background: #c9184a;
}

.card {
  background: #caffbf;
  margin: 15px auto;
  padding: 15px;
  width: 300px;
  border-radius: 10px;
  border-left: 6px solid green;
}
</style>