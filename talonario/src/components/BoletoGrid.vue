<template>
  <div class="boletos-grid">
    <button v-for="boleto in boletas" :key="boleto.numero" class="boleto-numero" :class="boleto.estado"
      @click="abrirEditor(boleto.numero)">
      {{ boleto.numero }}
    </button>
  </div>

  <!-- Modal Bootstrap -->
  <div class="modal fade show" tabindex="-1" role="dialog" v-show="mostrarEditor"
    style="display: block; background-color: rgba(0, 0, 0, 0.5);">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">
            Datos del cliente - Boleta #{{ boletaSeleccionada.numero }}
          </h5>
          <button type="button" class="btn-close" @click="cancelarEdicion"></button>
        </div>
        <div class="modal-body">
          <div class="mb-3">
            <input type="text" class="form-control" v-model="boletaSeleccionada.nombre"
              placeholder="Nombre y Apellido" />
          </div>

          <div class="mb-3">
            <input type="text" class="form-control" v-model="boletaSeleccionada.direccion" placeholder="Dirección" />
          </div>

          <div class="mb-3">
            <input type="text" class="form-control" v-model="boletaSeleccionada.celular" placeholder="Celular" />
          </div>

          <div class="mb-3">
            <select class="form-select" v-model="boletaSeleccionada.estado">
              <option value="disponible">Disponible</option>
              <option value="apartado">Apartado</option>
              <option value="pagado">Pagado</option>
            </select>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-primary" @click="guardarEdicion">
            Guardar
          </button>
          <button type="button" class="btn btn-secondary" @click="cancelarEdicion">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { inject, computed, ref } from 'vue'

const talonarioInfo = inject('talonarioInfo')
const updateBoleta = inject('updateBoleta')
const showAlert = inject('showAlert')

const boletas = computed(() => {
  return Array.isArray(talonarioInfo.boletas) ? talonarioInfo.boletas : []
})

const mostrarEditor = ref(false)
const boletaSeleccionada = ref({})

function abrirEditor(numero) {
  const boleta = boletas.value.find((b) => b.numero === numero)
  boletaSeleccionada.value = { ...boleta }
  mostrarEditor.value = true
}

function guardarEdicion() {
  const { nombre, direccion, celular } = boletaSeleccionada.value

  if (!nombre || !direccion || !celular) {
    showAlert('Por favor, completa todos los campos antes de guardar.', 'warning')
    return
  }

  // Actualizar la boleta en el estado global
  updateBoleta(boletaSeleccionada.value.numero, boletaSeleccionada.value)
  
  mostrarEditor.value = false
  showAlert('Los datos del cliente guardados correctamente.', 'success')
}

function cancelarEdicion() {
  mostrarEditor.value = false
}
</script>

<style scoped>
.boletos-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(45px, 1fr));
  gap: 10px;
  margin-top: 20px;
  max-width: 600px;
  margin-inline: auto;
}

.boleto-numero {
  background-color: #71717155;
  color: white;
  border: 2px solid #000000c8;
  border-radius: 50%;
  width: 45px;
  height: 45px;
  font-weight: bold;
  font-size: 0.9rem;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.boleto-numero.disponible {
  background-color: #555;
}

.boleto-numero.apartado {
  background-color: #d9534f;
}

.boleto-numero.pagado {
  background-color: #5cb85c;
}

.modal-content {
  border-radius: 10px;
}

.form-control,
.form-select {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 8px 12px;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: 500;
}

.btn-primary {
  background-color: #0052cc;
  color: white;
}

.btn-primary:hover {
  background-color: #164a94;
}

.btn-secondary {
  background-color: #6c757d;
  color: white;
}

.btn-secondary:hover {
  background-color: #5a6268;
}

.btn-close {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
}

.mb-3 {
  margin-bottom: 1rem;
}

.modal-header {
  padding: 1rem;
  border-bottom: 1px solid #dee2e6;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-body {
  padding: 1rem;
}

.modal-footer {
  padding: 1rem;
  border-top: 1px solid #dee2e6;
  display: flex;
  gap: 10px;
  justify-content: flex-end;
}
</style>
