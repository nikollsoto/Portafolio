<template>
  <div class="actions-panel">
    <h2 class="panel-title">Acciones</h2>
    <div class="actions-content">
      <button 
        class="action-btn list-btn"
        @click="listarBoletas"
        :disabled="!hasTalonarioInfo"
      >
        <span class="btn-icon">📋</span>
        LISTAR BOLETAS
      </button>
      
      <button 
        class="action-btn personalize-btn"
        @click="personalizar"
        :disabled="!hasTalonarioInfo"
      >
        <span class="btn-icon">⚙️</span>
        PERSONALIZAR
      </button>
    </div>
  </div>
</template>

<script>
import { defineComponent, computed, inject, reactive } from 'vue'

export default defineComponent({
  name: 'ActionsPanel',
  setup() {
    const talonarioInfo = inject('talonarioInfo', () => reactive({
      premio: '',
      valorBoleta: '',
      loteria: '',
      cantidadBoletas: '',
      fecha: ''
    }))
    
    console.log('ActionsPanel - talonarioInfo:', talonarioInfo)
    
    const hasTalonarioInfo = computed(() => {
      try {
        if (!talonarioInfo || typeof talonarioInfo !== 'object') {
          return false
        }
        const values = Object.values(talonarioInfo)
        return values.some(value => value !== '' && value !== null && value !== undefined)
      } catch (error) {
        console.error('Error in hasTalonarioInfo computed:', error)
        return false
      }
    })
    
    const listarBoletas = () => {
      if (!hasTalonarioInfo.value) {
        alert('Primero debe configurar el talonario')
        return
      }
      alert('Listando boletas...')
      // Aquí iría la lógica para listar boletas
    }
    
    const personalizar = () => {
      if (!hasTalonarioInfo.value) {
        alert('Primero debe configurar el talonario')
        return
      }
      alert('Abriendo opciones de personalización...')
      // Aquí iría la lógica para personalizar
    }
    
    return {
      hasTalonarioInfo,
      listarBoletas,
      personalizar
    }
  }
})
</script>

<style scoped>
.actions-panel {
  background-color: #d0d0d0;
  border-radius: 10px;
  padding: 20px;
  height: fit-content;
}

.panel-title {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 20px;
  color: #333;
  text-align: center;
}

.actions-content {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.action-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 12px 16px;
  border: none;
  border-radius: 5px;
  font-size: 0.9rem;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
  color: white;
}

.action-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.list-btn {
  background-color: #1e5aa8;
}

.list-btn:hover:not(:disabled) {
  background-color: #164a94;
}

.personalize-btn {
  background-color: #1e5aa8;
}

.personalize-btn:hover:not(:disabled) {
  background-color: #164a94;
}

.btn-icon {
  font-size: 1.1rem;
}

.action-btn:active:not(:disabled) {
  transform: translateY(1px);
}
</style>
