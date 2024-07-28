<script setup>

  import {ref, reactive, onMounted, watch} from "vue"

  import {uid} from "uid" // libreria externa instalada en el v94

  import Header from "./components/Header.vue"
  import Formulario from "./components/Formulario.vue"
  import Paciente from "./components/Paciente.vue"

  const pacientes = ref([])

  // watch, funcion nativa de vue, escucha por cambios en el state pacientes y cuando eso pasa actualiza "pacientes" en localStorage (v100)
  watch(pacientes, () => {
    guardarLocalStorage()
  }, {
    deep: true
  })

  onMounted(() => {
    const pacientesStorage = localStorage.getItem("pacientes")
    if(pacientesStorage) {
      pacientes.value = JSON.parse(pacientesStorage)
    }
  })
  const guardarLocalStorage = () => {
    localStorage.setItem("pacientes", JSON.stringify(pacientes.value))
  }

  const paciente = reactive({
      id: null,
      nombre: "", 
      propietario: "", 
      email: "", 
      alta: "", 
      sintomas: "", 
    })

  const guardarPaciente = () => {
    if(paciente.id){
      const {id} = paciente
      const i = pacientes.value.findIndex(paciente => paciente.id === id)
      pacientes.value[i] = {...paciente};
    } else {
      pacientes.value.push({...paciente, id: uid()})
    }
    
    // forma #1 de resetear un objeto (v92)
    // paciente.nombre = ""
    // paciente.propietario = ""
    // paciente.email = ""
    // paciente.alta = ""
    // paciente.sintomas = ""

    // forma #2 de resetear un objeto (v92)
    Object.assign(paciente, {
      nombre: "", 
      propietario: "", 
      email: "", 
      alta: "", 
      sintomas: "", 
      id: null
    })

  }

  const actualizarPaciente = (id) => { // custom event
    const pacienteEditar = pacientes.value.filter( paciente => paciente.id === id)[0];
    Object.assign(paciente, pacienteEditar)
  }

  const eliminarPaciente = (id) => { // custom event
    pacientes.value = pacientes.value.filter( paciente => paciente.id !== id);
  }

</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario 
        v-model:nombre="paciente.nombre" 
        v-model:propietario="paciente.propietario" 
        v-model:email="paciente.email"  
        v-model:alta="paciente.alta" 
        v-model:sintomas="paciente.sintomas" 
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus Pacientes</h3>
        <div v-if="pacientes.length">
          <p class="text-lg text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
          <Paciente 
            v-for="(paciente, i) in pacientes"
            :key="i"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div> 
        <p 
          class="mt-20 text-2xl text-center" 
          v-else 
        >No Hay Pacientes</p>
      </div>
    </div>

  </div>
</template>