<template>
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" :width="size" :height="size" :fill="color">
    <path d="M4,8h4V4H4V8z M10,20h4v-4h-4V20z M4,20h4v-4H4V20z M4,14h4v-4H4V14z M10,14h4v-4h-4V14z M16,4v4h4V4H16z M10,8h4V4h-4V8z M16,14h4v-4h-4V14z M16,20h4v-4h-4V20z" />
  </svg>
</template>

<script setup>
defineProps({
  size: {
    type: [String, Number],
    default: 24
  },
  color: {
    type: String,
    default: 'currentColor'
  }
})
</script> 