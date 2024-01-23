<script setup>
import { onMounted, ref } from 'vue'
const users = ref([])

onMounted(async () => {
  try {
    const response = await fetch('https://randomuser.me/api/?results=50')
    const data = await response.json()
    users.value = data.results
  } catch (error) {
    console.error('Error fetching data:', error)
  }
  return {
    users
  }
})

// Return the reactive state for use in the template
</script>

<template>
  <div>
    <table>
      <tr>
        <th>slno</th>
        <th>name</th>
        <th>DOB</th>
        <th>Email</th>
        <th>Location</th>
        <th>Phone</th>
        <th>Picture</th>
      </tr>
      <tr v-for="(user, index) in users" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ user.name.first }} {{ user.name.last }}</td>
        <td>{{ user.dob.date }}</td>
        <td>{{ user.email }}</td>
        <td>
          {{ user.location.street.number }}, {{ user.location.street.name }},
          {{ user.location.city }}, {{ user.location.state }}, {{ user.location.country }},
          {{ user.location.postcode }}, {{ user.location.coordinates.latitude }},
          {{ user.location.coordinates.longitude }}
        </td>
        <td>{{ user.phone }}</td>
        <td><img :src="user.picture.thumbnail" :alt="user.name.first" /></td>
      </tr>
    </table>
  </div>
</template>

<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
