<template>
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" :width="size" :height="size" :fill="color">
    <path d="M21.71,11.29l-9-9c-0.39-0.39-1.02-0.39-1.41,0l-9,9c-0.39,0.39-0.39,1.02,0,1.41l9,9c0.39,0.39,1.02,0.39,1.41,0l9-9 C22.1,12.32,22.1,11.69,21.71,11.29z M14,14.5V12h-4v3H8v-4c0-0.55,0.45-1,1-1h5V7.5l3.5,3.5L14,14.5z" />
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