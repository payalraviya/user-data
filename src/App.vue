<script setup>
import { onMounted, ref, computed } from 'vue'
import CustomTable from '@/components/CustomTable.vue'

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

const flattenLocation = (location) => {
  return `${location.street.number}, ${location.street.name}, ${location.street.name}, ${location.city}, ${location.state}, ${location.country}, ${location.postcode}, ${location.coordinates.latitude}, ${location.coordinates.longitude}`
}

const flattenName = (name) => {
  return `${name.first} ${name.last}`
}

const flattenedUsers = computed(() => {
  return users.value.map((user) => {
    const flattenedUser = {}

    flattenedUser.name = flattenName(user.name)
    flattenedUser.DOB = user.dob.date
    flattenedUser.email = user.email

    flattenedUser.location = flattenLocation(user.location)

    flattenedUser.phone = user.phone
    flattenedUser.picture = user.picture.thumbnail

    return flattenedUser
  })
})
</script>

<template>
  <div>
    <CustomTable :flattenedUsers="flattenedUsers" />
  </div>
</template>
