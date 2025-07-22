<template>
  <div class="form-container">
    <div class="form-header">
      <h2 class="form-title">CONFIGURA TU TALONARIO</h2>
      <button class="close-btn" @click="resetForm">×</button>
    </div>

    <form @submit.prevent="guardarTalonario" class="talonario-form">
      <div class="form-group">
        <input v-model="formData.premio" type="text" placeholder="Ingrese el premio de la rifa" class="form-input"
          required />
      </div>

      <div class="form-group">
        <input v-model="formData.valorBoleta" type="number" placeholder="Ingrese valor de la boleta" class="form-input"
          min="0" step="0.01" required />
      </div>

      <div class="form-group">
        <select v-model="formData.loteria" class="form-select" required>
          <option value="">Seleccione la lotería</option>
          <option value="Baloto">Baloto</option>
          <option value="Chance">Chance</option>
          <option value="Lotería de Boyacá">Lotería de Boyacá</option>
          <option value="Lotería de Cundinamarca">Lotería de Cundinamarca</option>
          <option value="Lotería de Medellín">Lotería de Medellín</option>
          <option value="Lotería de Bogotá">Lotería de Bogotá</option>
          <option value="Lotería de Antioquia">Lotería de Antioquia</option>
        </select>
      </div>

      <div class="form-group">
        <select v-model="formData.cantidadBoletas" class="form-select" required>
          <option value="">Cantidad de Boletas</option>
          <option value="0-99">0-99</option>
          <option value="0-200">0-999 </option>

        </select>
      </div>

      <div class="form-group">
        <input v-model="formData.fecha" type="date" class="form-input date-input"
          required />
      </div>

      <button type="submit" class="submit-btn">
        Guardar
      </button>
    </form>
  </div>
</template>

<script>
import { defineComponent, reactive, inject } from 'vue'

export default defineComponent({
  name: 'TalonarioForm',
  setup() {
    const updateTalonarioInfo = inject('updateTalonarioInfo', () => {
      console.warn('updateTalonarioInfo function not found')
    })

    console.log('TalonarioForm - updateTalonarioInfo:', updateTalonarioInfo)

    const formData = reactive({
      premio: '',
      valorBoleta: '',
      loteria: '',
      cantidadBoletas: '',
      fecha: ''
    })

    const guardarTalonario = () => {
      updateTalonarioInfo(formData)
      alert('Talonario guardado exitosamente!')
    }

    const resetForm = () => {
      Object.keys(formData).forEach(key => {
        formData[key] = ''
      })
    }

    return {
      formData,
      guardarTalonario,
      resetForm
    }
  }
})
</script>

<style scoped>
.form-container {
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.form-header {
  background-color: #1e5aa8;
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
  background-color: #1e5aa8;
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