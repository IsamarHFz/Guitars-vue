<script setup>
import { ref } from 'vue'

defineProps({
  name: String,
  msg: String,
})

const count = ref(0)
</script>

<template>
  <h1>{{ name }} dice {{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    
  </div>

</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
