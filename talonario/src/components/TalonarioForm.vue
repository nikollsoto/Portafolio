<template>
  <div class="form-container">
    <div class="form-header">
      <h2 class="form-title">CONFIGURA TU TALONARIO</h2>
      <button class="close-btn" @click="resetForm">×</button>
    </div>

    <form @submit.prevent="handleSubmit" class="talonario-form">
      <div class="form-group">
        <input v-model="formData.premio" type="number" min="1" placeholder="Ingrese el premio de la rifa"
          class="form-input" required />
      </div>

      <div class="form-group">
        <input v-model="formData.valorBoleta" type="number" min="1" placeholder="Ingrese valor de la boleta"
          class="form-input" required />
      </div>

      <div class="form-group">
        <select v-model="formData.loteria" class="form-select" required>
          <option value="">Seleccione la lotería</option>
          <option value="Lotería de Boyacá">Lotería de Boyacá</option>
          <option value="Lotería de Cundinamarca">Lotería de Cundinamarca</option>
          <option value="Lotería de Antioquia">Lotería de Antioquia</option>
          <option value="Lotería de Quindio">Lotería del Quindio</option>
          <option value="Lotería de Santander">Lotería de Santander</option>
        </select>
      </div>

      <div class="form-group">
        <select v-model="formData.cantidadBoletas" class="form-select" required>
          <option value="">Cantidad de Boletas</option>
          <option value="0-99">0-99</option>
          <option value="0-999">0-999</option>
        </select>
      </div>

      <div class="form-group">
        <input v-model="formData.fecha" type="date" class="form-input date-input" :min="today" required />
      </div>

      <button type="submit" class="submit-btn">
        {{ talonarioInfo.cantidadBoletas ? 'Actualizar' : 'Guardar' }}
      </button>
    </form>
  </div>
</template>

<script setup>
import { reactive, inject, computed, onMounted } from 'vue'
import Swal from 'sweetalert2'

const talonarioInfo = inject('talonarioInfo')
const updateTalonarioInfo = inject('updateTalonarioInfo')

// Fecha mínima (hoy)
const today = computed(() => {
  const d = new Date()
  return `${d.getFullYear()}-${String(d.getMonth() + 1).padStart(2, '0')}-${String(d.getDate()).padStart(2, '0')}`
})

const formData = reactive({
  premio: '',
  valorBoleta: '',
  loteria: '',
  cantidadBoletas: '',
  fecha: ''
})

const showAlert = (title, icon = 'success') => {
  Swal.fire({
    title,
    icon,
    confirmButtonColor: '#1e5aa8',
    confirmButtonText: 'OK'
  })
}

// Cargar datos si estamos editando
onMounted(() => {
  if (talonarioInfo.premio) {
    formData.premio = talonarioInfo.premio
    formData.valorBoleta = talonarioInfo.valorBoleta
    formData.loteria = talonarioInfo.loteria
    formData.cantidadBoletas = talonarioInfo.cantidadBoletas
    formData.fecha = talonarioInfo.fecha
  }
})

const handleSubmit = () => {
  if (!formData.premio || !formData.valorBoleta || !formData.loteria || !formData.cantidadBoletas || !formData.fecha) {
    showAlert('Todos los campos son obligatorios.', 'warning')
    return
  }

  if (formData.fecha < today.value) {
    showAlert('La fecha no puede ser anterior a hoy.', 'error')
    return
  }

  if (Number(formData.valorBoleta) > Number(formData.premio)) {
    showAlert('El valor de la boleta no puede ser mayor al premio.', 'error')
    return
  }

  try {
    const datos = {
      ...formData,
      premio: Number(formData.premio),
      valorBoleta: Number(formData.valorBoleta)
    }

    updateTalonarioInfo(datos)
    showAlert(talonarioInfo.premio ? 'Datos actualizados correctamente' : 'Talonario guardado exitosamente', 'success')
  } catch (error) {
    showAlert('Error al procesar los datos', 'error')
  }
}

// Función para resetear el formulario
const resetForm = () => {
  Object.assign(formData, {
    premio: '',
    valorBoleta: '',
    loteria: '',
    cantidadBoletas: '',
    fecha: ''
  })
}
</script>

<style scoped>
.form-container {
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.form-header {
  background-color: #299E1CFF;
  color: white;
  padding: 15px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.form-title {
  font-size: 1.1rem;
  font-weight: bold;
  margin: 0;
}

.close-btn {
  background: none;
  border: none;
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.close-btn:hover {
  background-color: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
}

.talonario-form {
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-input,
.form-select {
  padding: 12px;
  border: 2px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.form-input:focus,
.form-select:focus {
  outline: none;
  border-color: #1e5aa8;
}

.date-input {
  color: #666;
}

.submit-btn {
  background-color: #185D10FF;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

.submit-btn:hover {
  background-color: #164a94;
}

.submit-btn:active {
  transform: translateY(1px);
}
</style>
