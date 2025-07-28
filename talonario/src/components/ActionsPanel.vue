<template>
  <div class="actions-panel">
    <h2 class="panel-title">Acciones</h2>
    <div class="actions-content">
      <button class="action-btn list-btn" @click="listarBoletas" :disabled="!hasTalonarioInfo">
        <span class="btn-icon">LISTAR BOLETAS 📋</span>
      </button>


    </div>

    <!-- Modal  listar boletas -->
    <div class="modal fade show d-block" tabindex="-1" role="dialog" v-if="mostrarModal"
      style="background-color: rgba(0, 0, 0, 0.5);">
      <div class="modal-dialog modal-dialog-scrollable modal-lg" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title"> Clientes de Boletas</h5>
            <button type="button" class="btn-close" @click="cerrarModal">x</button>
          </div>

          <div class="modal-body">
            <div class="tabla-container" ref="tablaContainer">
              <div class="tabla-header">
                <h3>Listado </h3>
              </div>

              <div class="tabla-wrapper" ref="tabla">
                <table class="tabla-boletas">
                  <thead>
                    <tr>
                      <th># </th>
                      <th>Nombre</th>
                      <th>Dirección</th>
                      <th>Telefóno</th>
                      <th>Fecha</th>
                      <th>Estado</th>
                      
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="b in boletasConDatos" :key="b.numero">

                      <td>{{ b.numero }}</td>
                      <td>{{ b.nombre }}</td>
                      <td>{{ b.direccion }}</td>
                      <td>{{ b.celular }}</td>
                      <td>{{ formatearFecha(new Date()) }}</td>
                      <td><span :class="'estado-tag ' + b.estado">{{ capitalize(b.estado) }}</span></td>
                      
                    </tr>
                  </tbody>
                </table>
              </div>

              <div class="totales">
                <p><strong>Dinero recaudado: $ {{ formatearNumero(totalRecaudado) }}</strong></p>
                <p><strong>Deuda total: $ {{ formatearNumero(totalPendiente) }}</strong></p>
              </div>

              <div class="acciones-tabla">
                <button class="btn" @click="descargarPDF">Descargar PDF</button>
                <button class="btn" @click="cerrarModal">Cerrar</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, inject, computed } from 'vue'

const mostrarModal = ref(false)
const tabla = ref(null)
const tablaContainer = ref(null)
const talonarioInfo = inject('talonarioInfo')

const hasTalonarioInfo = computed(() => {
  if (!talonarioInfo || typeof talonarioInfo !== 'object') return false
  const values = Object.values(talonarioInfo).filter(v => typeof v !== 'function')
  return values.some(value => value !== '' && value !== null && value !== undefined)
})

const valorBoleta = computed(() => parseFloat(talonarioInfo.valorBoleta || 0))

const boletas = computed(() => {
  return Array.isArray(talonarioInfo.boletas) ? talonarioInfo.boletas : []
})

const boletasConDatos = computed(() => {
  return boletas.value.filter(b => b.nombre && b.direccion && b.celular)
})

const totalRecaudado = computed(() => {
  return boletas.value.filter(b => b.estado === 'pagado').length * valorBoleta.value
})

const totalPendiente = computed(() => {
  return boletas.value.filter(b => b.estado === 'apartado').length * valorBoleta.value
})

function listarBoletas() {
  if (!hasTalonarioInfo.value) {
    alert('Primero debe configurar el talonario')
    return
  }
  mostrarModal.value = true
}

function cerrarModal() {
  mostrarModal.value = false
}


function formatearFecha(fecha) {
  return fecha.toLocaleDateString('es-CO')
}

function formatearNumero(numero) {
  return new Intl.NumberFormat('es-CO').format(numero)
}

function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1)
}

function descargarPDF() {
  if (!tablaContainer.value) return

  //  HTML para el PDF
  const contenido = `
    <div style="font-family: Arial, sans-serif; padding: 20px;">
      <div style="background-color: #1e5aa8; color: white; padding: 20px; text-align: center; margin-bottom: 20px;">
        <h1 style="margin: 0; font-size: 2rem; letter-spacing: 2px;">Listado de Boleta</h1>
      </div>
      
      <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
        <thead style="background-color: #d4a574;">
          <tr>
            <th style="border: 1px solid #000; padding: 8px; text-align: center;">Nombre Comprador</th>
            <th style="border: 1px solid #000; padding: 8px; text-align: center;">Dirección</th>
            <th style="border: 1px solid #000; padding: 8px; text-align: center;">Número Telefónico</th>
            <th style="border: 1px solid #000; padding: 8px; text-align: center;">Fecha Compra Boleta</th>
            <th style="border: 1px solid #000; padding: 8px; text-align: center;">Estado Boleta</th>
            <th style="border: 1px solid #000; padding: 8px; text-align: center;">N. Boleta</th>
          </tr>
        </thead>
        <tbody>
          ${boletasConDatos.value.map(b => `
            <tr>
              <td style="border: 1px solid #000; padding: 8px;">${b.nombre}</td>
              <td style="border: 1px solid #000; padding: 8px;">${b.direccion}</td>
              <td style="border: 1px solid #000; padding: 8px;">${b.celular}</td>
              <td style="border: 1px solid #000; padding: 8px;">${formatearFecha(new Date())}</td>
              <td style="border: 1px solid #000; padding: 8px;">${capitalize(b.estado)}</td>
              <td style="border: 1px solid #000; padding: 8px;">${b.numero}</td>
            </tr>
          `).join('')}
        </tbody>
      </table>
      
      <div style="text-align: center; margin-top: 20px;">
        <p style="font-size: 1.2rem;"><strong>Dinero recaudado: $ ${formatearNumero(totalRecaudado.value)}</strong></p>
        <p style="font-size: 1.2rem;"><strong>Deuda total: $ ${formatearNumero(totalPendiente.value)}</strong></p>
      </div>
    </div>
  `

  // Crear un elemento temporal para convertir a PDF
  const elementoTemporal = document.createElement('div')
  elementoTemporal.innerHTML = contenido

  // Simular la descarga (en un entorno real usarías una librería como jsPDF o html2pdf)
  const blob = new Blob([contenido], { type: 'text/html' })
  const url = URL.createObjectURL(blob)
  const a = document.createElement('a')
  a.href = url
  a.download = 'listado-boletas.html'
  document.body.appendChild(a)
  a.click()
  document.body.removeChild(a)
  URL.revokeObjectURL(url)

  // Mostrar alerta de éxito
  alert('PDF descargado exitosamente')
}
</script>

<style scoped>
.actions-panel {
  background-color: #e7e7e7;
  border-radius: 10px;
  padding: 20px;
  height: fit-content;
}

.panel-title {
  font-size: 1.9rem;
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
  background-color: #0052cc;
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

.modal-content {
  border-radius: 30px;
  background-color: #ffffff;
  font-size: 0.85rem;
}

.modal-header {
  background-color: #16700CDD;
  color: white;
  padding: 15px 20px;
  border-radius: 10px 10px 0 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-title {
  margin: 0;
  font-size: 1.2rem;
  font-weight: bold;
  color: white;
}

.btn-close {
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.454);
  font-size: 1.5rem;
  cursor: pointer;
  width: 50px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn-close:hover {
  background-color: rgba(0, 0, 0, 0.417);
  border-radius: 50%;

}

.tabla-container {
  padding: 20px;
}

.tabla-header {
  text-align: center;
  margin-bottom: 20px;
}

.tabla-header h3 {
  color: #333;
  font-size: 1.5rem;
  margin: 0;
}

.tabla-wrapper {
  overflow-x: auto;
  margin-bottom: 20px;
}

.tabla-boletas {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.9rem;
}

.tabla-boletas thead {
  background-color: #888888BB;
}

.tabla-boletas th,
.tabla-boletas td {
  border: 1px solid #000;
  padding: 8px;
  text-align: left;
}

.tabla-boletas th {
  font-weight: bold;
  text-align: center;
}

.estado-tag {
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 0.8rem;
  font-weight: bold;
  text-transform: uppercase;
}

.estado-tag.disponible {
  background-color: #6c757d;
  color: white;
}

.estado-tag.apartado {
  background-color: #dc3545;
  color: white;
}

.estado-tag.pagado {
  background-color: #28a745;
  color: white;
}

.totales {
  text-align: center;
  margin: 20px 0;
  font-size: 1.1rem;
  color: #333;
}

.totales p {
  margin: 5px 0;
}

.acciones-tabla {
  text-align: center;
  margin-top: 20px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn-primary {
  background-color: #0052cc;
  color: white;
}

.btn-primary:hover {
  background-color: #164a94;
}
</style>
