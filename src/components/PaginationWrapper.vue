<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps(['pagination'])

const { next, prev, canNext, canPrev, currentPage, pageCount, toPage } = props.pagination
</script>

<template>
  <div class="text-left">
    <div class="flex flex-row items-center space-x-1">
      <slot name="header"></slot>
    </div>
    <div class="my-4">
      <slot></slot>
    </div>
    <div>
      Page {{ currentPage }} of {{ pageCount }}
    </div>
    <div class="mt-4 btn-group">
      <button class="btn" :class="{'opacity-50 cursor-not-allowed': !canPrev}" @click="canPrev && prev()">
        Previous
      </button>
      <PaginationButton v-for="page in pageCount" :key="page" :page="page" :current-page="currentPage" @click="toPage(page)" />
      <button class="btn" :class="{'opacity-50 cursor-not-allowed': !canNext}" @click="canNext && next()">
        Next
      </button>
    </div>
  </div>
</template>
