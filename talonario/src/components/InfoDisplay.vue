<template>
  <div class="info-display">
    <div class="info-header">
      <h2>Información del Talonario</h2>
      <button @click="onEdit" class="edit-btn">
        <span class="material-icons">edit</span>
        Editar
      </button>
    </div>
    
    <div class="info-content">
      <div class="info-item">
        <label>Premio:</label>
        <span>{{ talonarioInfo.premio }}</span>
      </div>
      <div class="info-item">
        <label>Valor Boleta:</label>
        <span>${{ talonarioInfo.valorBoleta }}</span>
      </div>
      <div class="info-item">
        <label>Lotería:</label>
        <span>{{ talonarioInfo.loteria }}</span>
      </div>
      <div class="info-item">
        <label>Fecha:</label>
        <span>{{ talonarioInfo.fecha }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { inject } from 'vue'

const talonarioInfo = inject('talonarioInfo')
const showForm = inject('showForm')

const onEdit = () => {
  showForm()
}
</script>

<style scoped>
.info-display {
  background-color: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.info-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.edit-btn {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 8px 16px;
  background-color: #1e5aa8;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.edit-btn:hover {
  background-color: #164a94;
}

.info-content {
  display: grid;
  gap: 15px;
}

.info-item {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.info-item label {
  font-weight: bold;
  color: #666;
}

.info-item span {
  font-size: 1.1rem;
}
</style>