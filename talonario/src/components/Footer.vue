<template>
  <footer class="footer">
    <p class="footer-text">
      Copyright ©2024. Todos los derechos reservados.
    </p>
  </footer>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'Footer'
})
</script>

<style scoped>
.footer {
  background-color: #1e5aa8;
  color: white;
  padding: 15px;
  text-align: center;
}

.footer-text {
  font-size: 0.9rem;
}
</style>