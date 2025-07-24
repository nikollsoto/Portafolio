<script setup>
import { inject, computed } from 'vue'

const talonarioInfo = inject('talonarioInfo')

const totalBoletas = computed(() => {
  // Determinar el máximo número basado en la opción seleccionada
  const maxNumber = talonarioInfo.cantidadBoletas === '0-999' ? 999 : 99
  // Crear array desde 0 hasta el máximo número
  return Array.from({ length: maxNumber + 1 }, (_, i) => i)
})
</script>

<template>
  <div class="boletos-grid">
    <button 
      v-for="numero in totalBoletas" 
      :key="numero"
      class="boleto-numero"
    >
      {{ numero }}
    </button>
  </div>
</template>

<style scoped>
.boletos-container {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.boletos-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(45px, 1fr));
  gap: 10px;
  justify-items: center;
}

.boleto-numero {
  width: 40px;
  height: 40px;
  background-color: #0052cc;
  color: white;
  border: none;
  border-radius: 50%;
  font-size: 14px;
  font-weight: bold;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.boleto-numero:hover {
  background-color: #0043a4;
}

@media (max-width: 768px) {
  .boletos-grid {
    grid-template-columns: repeat(auto-fill, minmax(35px, 1fr));
    gap: 8px;
  }

  .boleto-numero {
    width: 35px;
    height: 35px;
    font-size: 12px;
  }
}
</style>