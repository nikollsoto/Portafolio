<template>
  <div id="app">
    <Header />
    <main class="main-container">
      <div class="layout">
        <InfoPanel />
        <TalonarioForm />
        <ActionsPanel />
      </div>
    </main>
    <Footer />
  </div>
</template>

<script>
import { defineComponent, reactive, provide } from 'vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import InfoPanel from './components/InfoPanel.vue'
import TalonarioForm from './components/TalonarioForm.vue'
import ActionsPanel from './components/ActionsPanel.vue'


export default defineComponent({
  name: 'App',
  components: {
    Header,
    Footer,
    InfoPanel,
    TalonarioForm,
    ActionsPanel
  },
  setup() {
    // Estado reactivo para la información del talonario
    const talonarioInfo = reactive({
      premio: '',
      valorBoleta: '',
      loteria: '',
      cantidadBoletas: '',
      fecha: ''
    })
    
    console.log('App - talonarioInfo created:', talonarioInfo)
    
    // Función para actualizar la información del talonario
    const updateTalonarioInfo = (newInfo) => {
      console.log('App - updating talonarioInfo with:', newInfo)
      Object.assign(talonarioInfo, newInfo)
      console.log('App - talonarioInfo after update:', talonarioInfo)
    }
    
    // Proporcionar el estado y la función a los componentes hijos
    provide('talonarioInfo', talonarioInfo)
    provide('updateTalonarioInfo', updateTalonarioInfo)
    
    console.log('App - provided talonarioInfo and updateTalonarioInfo')
    
    return {}
  }
})
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

@media (max-width: 768px) {
  .layout {
    grid-template-columns: 1fr;
    gap: 15px;
  }
}
</style>