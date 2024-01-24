<script setup>
import { onMounted, ref, computed } from 'vue'
const users = ref([])
const ascending = ref(false)
const sortColumn = ref('')

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
const sortTable = (col) => {
  if (sortColumn.value === col) {
    ascending.value = !ascending.value
  } else {
    ascending.value = true
    sortColumn.value = col
  }

  const ascendingValue = ascending.value
  if (col == 'name') {
    users.value.sort((a, b) => {
      console.log(a[col])
      if (a[col].first > b[col].first) {
        return ascendingValue ? 1 : -1
      } else if (a[col].first < b[col].first) {
        return ascendingValue ? -1 : 1
      }
      return 0
    })
  } else if (col == 'dob') {
    users.value.sort((a, b) => {
      console.log(a[col])
      if (a[col].date > b[col].date) {
        return ascendingValue ? 1 : -1
      } else if (a[col].date < b[col].date) {
        return ascendingValue ? -1 : 1
      }
      return 0
    })
  } else if (col == 'email') {
    users.value.sort((a, b) => {
      console.log(a[col])
      if (a[col] > b[col]) {
        return ascendingValue ? 1 : -1
      } else if (a[col] < b[col]) {
        return ascendingValue ? -1 : 1
      }
      return 0
    })
  } else if (col == 'location') {
    users.value.sort((a, b) => {
      console.log(a[col])
      if (a[col].street.number > b[col].street.number) {
        return ascendingValue ? 1 : -1
      } else if (a[col].street.number < b[col].street.number) {
        return ascendingValue ? -1 : 1
      }
      return 0
    })
  } else if (col == 'phone') {
    users.value.sort((a, b) => {
      console.log(a[col])
      if (a[col] > b[col]) {
        return ascendingValue ? 1 : -1
      } else if (a[col] < b[col]) {
        return ascendingValue ? -1 : 1
      }
      return 0
    })
  }
}

const columns = computed(() => {
  if (users.value.length === 0) {
    return []
  }
  return Object.keys(users.value[0])
})
</script>

<template>
  <div>
    <table>
      <tr>
        <th @click="sortTable('slno')">
          slno
          <div class="arrow" v-bind:class="ascending ? 'arrow_up' : 'arrow_down'"></div>
        </th>
        <th @click="sortTable('name')">
          name
          <div class="arrow" v-bind:class="ascending ? 'arrow_up' : 'arrow_down'"></div>
        </th>
        <th @click="sortTable('dob')">
          DOB
          <div class="arrow" v-bind:class="ascending ? 'arrow_up' : 'arrow_down'"></div>
        </th>
        <th @click="sortTable('email')">
          Email
          <div class="arrow" v-bind:class="ascending ? 'arrow_up' : 'arrow_down'"></div>
        </th>
        <th @click="sortTable('location')">
          Location
          <div class="arrow" v-bind:class="ascending ? 'arrow_up' : 'arrow_down'"></div>
        </th>
        <th @click="sortTable('phone')">
          Phone
          <div class="arrow" v-bind:class="ascending ? 'arrow_up' : 'arrow_down'"></div>
        </th>
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

.arrow_down {
  background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB8AAAAaCAYAAABPY4eKAAAAAXNSR0IArs4c6QAAAvlJREFUSA29Vk1PGlEUHQaiiewslpUJiyYs2yb9AyRuJGm7c0VJoFXSX9A0sSZN04ULF12YEBQDhMCuSZOm1FhTiLY2Rky0QPlQBLRUsICoIN/0PCsGyox26NC3eTNn3r3n3TvnvvsE1PkwGo3yUqkkEQqFgw2Mz7lWqwng7ztN06mxsTEv8U0Aam5u7r5EInkplUol/f391wAJCc7nEAgE9Uwmkzo4OPiJMa1Wq6cFs7Ozt0H6RqlUDmJXfPIx+qrX69Ti4mIyHA5r6Wq1egND+j+IyW6QAUoul18XiUTDNHaSyGazKcZtdgk8wqhUKh9o/OMvsVgsfHJy0iWqVrcQNRUMBnd6enqc9MjISAmRP3e73T9al3XnbWNjIw2+KY1Gc3imsNHR0YV4PP5+d3e32h3K316TySQFoX2WyWR2glzIO5fLTSD6IElLNwbqnFpbWyO/96lCoai0cZjN5kfYQAYi5H34fL6cxWIZbya9iJyAhULBHAqFVlMpfsV/fHxMeb3er+Vy+VUzeduzwWC45XA4dlD/vEXvdDrj8DvURsYEWK3WF4FA4JQP9mg0WrHZbEYmnpa0NxYgPVObm5teiLABdTQT8a6vrwdRWhOcHMzMzCiXlpb2/yV6qDttMpkeshEzRk4Wo/bfoe4X9vb2amzGl+HoXNT29vZqsVi0sK1jJScG+Xx+HGkL4Tew2TPi5zUdQQt9otPpuBk3e0TaHmMDh1zS7/f780S0zX6Yni+NnBj09fUZUfvudDrNZN+GkQbl8Xi8RLRtHzsB9Hr9nfn5+SjSeWUCXC7XPq5kw53wsNogjZNohYXL2EljstvtrAL70/mVaW8Y4OidRO1/gwgbUMvcqGmcDc9aPvD1gnTeQ+0nmaInokRj0nHh+uvIiVOtVvt2a2vLv7Ky0tL3cRTXIcpPAwMDpq6R4/JXE4vFQ5FI5CN+QTaRSFCYc8vLy1l0rge4ARe5kJ/d27kYkLXoy2Jo4C7K8CZOsEBvb+9rlUp1xNXPL7v3IDwxvPD6AAAAAElFTkSuQmCC');
}
.arrow_up {
  background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAB4AAAAaCAYAAACgoey0AAAAAXNSR0IArs4c6QAAAwpJREFUSA21Vt1PUmEYP4dvkQ8JFMwtBRocWAkDbiqXrUWXzU1rrTt0bdVqXbb1tbW16C9IBUSmm27cODdneoXjputa6069qwuW6IIBIdLvdaF4OAcOiGeDc87zPs/vd57P96WpFq7p6enbGo1mjKZpeTabjU1MTCRagGnOZHFxcXxtbe1XKpUq7+zslJeXl//Mz8+Hy+Uy3RxSE9qTk5M3otFooVQqgef4Wl9f343FYoEmoISrxuNxFX5f9vb2jhn/PxUKhfLS0tIPfFifUESRUMV8Pv/M6XReRm5rTGQyGeXxeGxYe1ezeBpBOBx2rKysbO7v79d4Wy3Y2Nj4GQqFbgnhaugxwiuGJx99Pp9FLBbXxYTXvTqd7v3MzIy6riIWGxJnMpl7AwMD14xGYyMsSq1WUyQdUqn0eSPlusQIsbGrq+vl4OCgvhFQZd1utyv1en0gEolcqsi47nWJlUrlG5fLZVcoFFy2nDKSDpIWlUoVTCQSEk4lCHmJMZ2GTCbTiMVikfIZ88l7enoos9l8dXt7+z6fDicxSJUokqDX6xXcl2wCROoc0vQCWL3sNfLOSdzR0fHY4XC4tVotl40gmVwup9xuN4OQv+UyqCFGH9rg7SOGYVRcBs3IEG4J0nVnamrqOtvuBDGGgQg9+wHFcVEi4a0LNkbdd6TrPKo8ODc311mteIIYjT/a398/jK+s1jnVM0kXoufCFvq0GuiIGEVgQIhfoygM1QrteEa9dAL7ITiYCt4RMabOK5AyKKzKWtvupLcRciu8D5J0EuDDPyT/Snd39yh6VtY2NhYQSR9G79Ds7OxdskRjEyAufvb7/cPoO5Z6e1+xtVKrq6vfcFzyi/A3ZrPZ3GdNSlwgo5ekE4X2RIQGf2C1WlufFE0GBeGWYQ8YERWLxQtnUVB830MKLZfL9RHir8lkssCn2G751tZWEWe03zTKm15YWPiEiXXTYDB0Ig/t7yd8PRws4EicwWHxO4jHD8/C5HiTTqd1BwcHFozKU89origB+y/kmzgYpgOBQP4fGmUiZmJ+WNgAAAAASUVORK5CYII=');
}
.arrow {
  float: right;
  width: 12px;
  height: 15px;
  background-repeat: no-repeat;
  background-size: contain;
  background-position-y: bottom;
}
</style>
