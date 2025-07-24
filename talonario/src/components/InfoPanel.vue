<template>
  <div class="info-panel">
    <h2 class="panel-title">Información del Talonario</h2>
    <div class="info-content" v-if="talonarioInfo.cantidadBoletas">
      <div class="info-item">
        <span>🏆</span>
        <strong><p class="info-value">$ {{ formatNumber(talonarioInfo.premio) }}</p></strong>
      </div>
      
      <div class="info-item">
        <span>💲</span>
        <strong><p class="info-value">$ {{ formatNumber(talonarioInfo.valorBoleta) }}</p></strong>
      </div>
      
      <div class="info-item">
        <span>🎰</span>
        <strong><p class="info-value">{{ talonarioInfo.loteria }}</p></strong>
      </div>
      
      <div class="info-item">
        <span>📅</span>
       <strong><p class="info-value">{{ formatDate(talonarioInfo.fecha) }}</p></strong>
      </div>

      <button class="edit-button" @click="handleEdit">
        <strong><span>Editar✍🏻</span></strong> 
      </button>
    </div>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const talonarioInfo = inject('talonarioInfo')
const showForm = inject('showForm')

const formatNumber = (number) => {
  return new Intl.NumberFormat('es-CO').format(number)
}

const formatDate = (date) => {
  return new Date(date).toLocaleDateString('es-CO')
}

const handleEdit = () => {
  showForm()
}
</script>

<style scoped>
.info-panel {
  background-color: #f8f9fa;
  padding: 1.5rem;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.panel-title {
  font-size: 1.5rem;
  color: #333;
  margin-bottom: 1.5rem;
  font-weight: bold;
}

.info-content {
  background-color: white;
  padding: 1.5rem;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.info-item span {
  font-size: 1.5rem;
  min-width: 30px;
}

.info-value {
  font-size: 1.1rem;
  color: #333;
  margin: 0;
  font-weight: 500;
}

.edit-button {
  margin-top: 1rem;
  padding: 0.5rem 1rem;
  background-color: #0052cc;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  width: fit-content;
}

.edit-button:hover {
  background-color: #0043a4;
}

.edit-icon {
  font-size: 1.2rem;
}
</style>