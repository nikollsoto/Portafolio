<template>
  <div class="info-panel">
    <h2 class="panel-title">Información del Talonario</h2>
    <div class="info-content">
      <div v-if="talonarioInfo && talonarioInfo.premio" class="info-item">
        <strong>Premio:</strong> {{ talonarioInfo.premio }}
      </div>
      <div v-if="talonarioInfo && talonarioInfo.valorBoleta" class="info-item">
        <strong>Valor de Boleta:</strong> ${{ talonarioInfo.valorBoleta }}
      </div>
      <div v-if="talonarioInfo && talonarioInfo.loteria" class="info-item">
        <strong>Lotería:</strong> {{ talonarioInfo.loteria }}
      </div>
      <div v-if="talonarioInfo && talonarioInfo.cantidadBoletas" class="info-item">
        <strong>Cantidad de Boletas:</strong> {{ talonarioInfo.cantidadBoletas }}
      </div>
      <div v-if="talonarioInfo && talonarioInfo.fecha" class="info-item">
        <strong>Fecha:</strong> {{ formatDate(talonarioInfo.fecha) }}
      </div>
      <div v-if="!hasInfo" class="no-info">
        No hay información del talonario configurada
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, computed, inject, reactive } from 'vue'

export default defineComponent({
  name: 'InfoPanel',
  setup() {
    const talonarioInfo = inject('talonarioInfo', () => reactive({
      premio: '',
      valorBoleta: '',
      loteria: '',
      cantidadBoletas: '',
      fecha: ''
    }))
    
    console.log('InfoPanel - talonarioInfo:', talonarioInfo)
    
    const hasInfo = computed(() => {
      try {
        if (!talonarioInfo || typeof talonarioInfo !== 'object') {
          return false
        }
        const values = Object.values(talonarioInfo)
        return values.some(value => value !== '' && value !== null && value !== undefined)
      } catch (error) {
        console.error('Error in hasInfo computed:', error)
        return false
      }
    })
    
    const formatDate = (date) => {
      if (!date) return ''
      return new Date(date).toLocaleDateString('es-ES')
    }
    
    return {
      talonarioInfo,
      hasInfo,
      formatDate
    }
  }
})
</script>

<style scoped>
.info-panel {
  background-color: #d0d0d0;
  border-radius: 10px;
  padding: 20px;
  height: fit-content;
}

.panel-title {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 15px;
  color: #333;
}

.info-content {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.info-item {
  padding: 8px 0;
  border-bottom: 1px solid #bbb;
}

.info-item:last-child {
  border-bottom: none;
}

.no-info {
  color: #666;
  font-style: italic;
  text-align: center;
  padding: 20px;
}
</style>