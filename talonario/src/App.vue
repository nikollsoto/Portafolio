<template>
  <div id="app">
    <Header />
    <main class="main-container">
      <!-- Bootstrap Alert -->
      <div v-if="alert.show" 
           :class="`alert alert-${alert.type} alert-dismissible fade show position-fixed top-0 start-50 translate-middle-x mt-3`" 
           role="alert"
           style="z-index: 1050;">
        {{ alert.message }}
        <button type="button" 
                class="btn-close" 
                @click="alert.show = false" 
                aria-label="Close"></button>
      </div>
      
      <div class="layout">
        <InfoPanel />
        <div class="content-section">
        
          <TalonarioForm v-if="!talonarioInfo.cantidadBoletas || showingForm" />
        
          <BoletoGrid v-else />
        </div>
        <ActionsPanel />
      </div>
    </main>
    <Footer />
  </div>
</template>

<script setup>
import { reactive, provide, ref } from 'vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import InfoPanel from './components/InfoPanel.vue'
import TalonarioForm from './components/TalonarioForm.vue'
import ActionsPanel from './components/ActionsPanel.vue'
import BoletoGrid from './components/BoletoGrid.vue'

// Estado reactivo para la información del talonario
const talonarioInfo = reactive({
  premio: '',
  valorBoleta: '',
  loteria: '',
  cantidadBoletas: '',
  fecha: ''
})

// Estado para las alertas
const alert = reactive({
  show: false,
  message: '',
  type: 'success'
})

// Función para mostrar alertas
const showAlert = (message, type = 'success') => {
  alert.message = message
  alert.type = type
  alert.show = true
  
  // Auto ocultar después de 3 segundos
  setTimeout(() => {
    alert.show = false
  }, 3000)
}

// Provide alert functionality
provide('showAlert', showAlert)

const showingForm = ref(false)

const showForm = () => {
  showingForm.value = true
}

// Función para actualizar la información del talonario
const updateTalonarioInfo = (newInfo) => {
  Object.assign(talonarioInfo, newInfo)
  showingForm.value = false // Oculta el formulario después de guardar
}

// Proporcionar el estado y la función a los componentes hijos
provide('talonarioInfo', talonarioInfo)
provide('updateTalonarioInfo', updateTalonarioInfo)
provide('showForm', showForm)
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: system-ui;
  background-color: #f0f0f0;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.main-container {
  flex: 1;
  padding: 20px;
}

.layout {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  gap: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.content-section {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

@media (max-width: 768px) {
  .layout {
    grid-template-columns: 1fr;
    gap: 15px;
  }
}
</style>
