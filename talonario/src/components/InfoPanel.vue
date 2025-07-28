<template>
  <div class="info-panel">
    <h2 class="panel-title">Información del Sorteo</h2>
    <div class="info-content" v-if="talonarioInfo.cantidadBoletas">
      <div class="info-item">
        <span>🏆</span>
        <p class="info-value"><strong>$ {{ formatNumber(talonarioInfo.premio) }}</strong></p>
      </div>

      <div class="info-item">
        <span>💲</span>
        <p class="info-value"><strong>$ {{ formatNumber(talonarioInfo.valorBoleta) }}</strong></p>
      </div>

      <div class="info-item">
        <span>🎰</span>
        <p class="info-value"><strong>{{ talonarioInfo.loteria }}</strong></p>
      </div>

      <div class="info-item">
        <span>📅</span>
        <p class="info-value"><strong>{{ formatDate(talonarioInfo.fecha) }}</strong></p>
      </div>

      <button class="edit-button" @click="handleEdit">
        <span><strong>Editar✍🏻</strong> </span>
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
  background-color: #e7e7e7;
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
</style>
