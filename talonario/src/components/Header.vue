<template>
  <header class="header">
    <h1 class="header-title">TALONARIO</h1>
  </header>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Header'
})
</script>

<style scoped>
.header {
  background-color: #1e5aa8;
  color: white;
  padding: 20px;
  text-align: center;
}

.header-title {
  font-size: 2rem;
  font-weight: bold;
  letter-spacing: 2px;
}
</style>