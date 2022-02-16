<template>
  <div class="w-36 form-control">
    <label class="label">
      <span class="label-text">Rows Per Page</span>
    </label>
    <select v-model="rowCount" class="select select-bordered">
      <option :value="5">
        5
      </option>
      <option :value="8">
        8
      </option>
      <option :value="10">
        10
      </option>
    </select>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps({
  rowsPerPage: { type: Number },
})
const emit = defineEmits(['update:rowsPerPage'])
const rowCount = computed({
  get() { return props.rowsPerPage },
  set(value) { emit('update:rowsPerPage', value) }
})
</script>
