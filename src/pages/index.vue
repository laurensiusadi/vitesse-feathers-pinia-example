<script setup lang="ts">
import { useFind } from 'feathers-pinia'
import { useUsers } from '~/stores/user'

const userStore = useUsers()

const selectedClass = ref(null)
const search = ref('')

const current = ref(null)
const setCurrent = (item: typeof userStore.Model) => current.value = item

const pagination = reactive({ $limit: 5, $skip: 0 })
const params = computed(() => {
  const nameFilter = { name: { $regex: search.value, $options: 'igm' } }
  const classFilter = { class: selectedClass.value }

  return {
    query: {
      ...pagination,
      ...(selectedClass.value ? classFilter : null),
      ...nameFilter,
    },
    paginate: true,
  }
})
const { items: users, latestQuery } = useFind({ model: userStore.Model, params })
</script>

<template>
  <pagination-wrapper :pagination="pagination" :latest-query="latestQuery">
    <template #header>
      <RowsPerPageSelector v-model:rows-per-page="pagination.$limit" />
      <ClassSelector v-model:school-class="selectedClass" />
      <TextInput v-model:value="search" label="Search by Name" placeholder="name" />
    </template>
    <!-- User Table -->
    <table class="table table-zebra w-130">
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
          <th>Class</th>
        </tr>
      </thead>
      <tbody>
        <UserRow v-for="user in users" :key="user.id" :user="user" :class="{ active: user === current }" @click="() => setCurrent(user)" />
      </tbody>
    </table>
  </pagination-wrapper>
</template>

<route lang="yaml">
meta:
  layout: home
</route>
