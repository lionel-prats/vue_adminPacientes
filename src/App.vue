<script setup>
  import {ref, reactive} from "vue"
  import Header from "./components/Header.vue"
  import Formulario from "./components/Formulario.vue"
  import Paciente from "./components/Paciente.vue"

  const pacientes = ref([/* {
    nombre: "Chiqui",
    propietario: "Lupi",
    email: "lupi@correo.com",
    alta: "2024-07-27",
    sintomas: "No quiere comer",
  } */])

  const paciente = reactive({
      nombre: "", 
      propietario: "", 
      email: "", 
      alta: "", 
      sintomas: "", 
    })

  const guardarPaciente = () => {
    pacientes.value.push({...paciente})
    
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
    })

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