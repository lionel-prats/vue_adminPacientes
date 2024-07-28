<script setup>
    import {/* ref */ reactive} from "vue"
    import Alerta from "./Alerta.vue"
    
    // const nombre = ref("Max"); // ej. v78
    // const propietario = ref("Lionel"); // ej. v78
    /* // ej. v78
    const setPropietario = e => {
        propietario.value = e.target.value
    } 
    */
    /* const props = defineProps({
        paciente: {
            type: Object,
            required: true,
        }
    }) */

    const emit = defineEmits(["update:nombre", "update:propietario", "update:email", "update:alta", "update:sintomas", "guardar-paciente"]) // v88

    const props = defineProps({ // v88
        nombre: {
            type: String,
            required: true,
        },
        propietario: {
            type: String,
            required: true,
        },
        email: {
            type: String,
            required: true,
        },
        alta: {
            type: String,
            required: true,
        },
        sintomas: {
            type: String,
            required: true,
        }
    })

    const alerta = reactive({
        "tipo": "",
        "mensaje": "",
    })

    const validar = () => {
        if(Object.values(props).includes("")){
            alerta.tipo = "error"
            alerta.mensaje = "Todos los campos son obligatorios"
            return
        } 
        emit("guardar-paciente")
        alerta.tipo = "exito"
        alerta.mensaje = "Paciente almacenado correctamente"
        setTimeout(() => {
            Object.assign(alerta, {
                "tipo": "",
                "mensaje": "",
            })
        }, 3000);
    }

</script>
<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
        <p class="text-lg text-center mb-10">
            Añade Pacientes y
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>
        <Alerta 
            v-if="alerta.mensaje"
            :alerta="alerta"
        />
        <form
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validar"    
        >
        <!-- v-on -->
            <div class="mb-5">
                <label
                    class="block text-gray-700 uppercase font-bold" 
                    for="mascota"
                >Nombre Mascota
                </label>
                <input
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
                    type="text"
                    id="mascota"
                    placeholder="Nombre de la mascota"
                    @input="$emit('update:nombre', $event.target.value)"
                    :value = "nombre"
                >
                <!-- v-model="nombre" (ej. v78) -->
                <!-- :value = "nombre" (ej. v78) -->
                <!-- @input = "(e) => nombre = e.target.value" (ej. v78) -->
                <!-- v-on:input="(e) => nombre = e.target.value" (ej. v78) -->
                <!-- @input="(e) => nombre = e.target.value" (ej. v78) -->
            </div>
            <div class="mb-5">
                <label
                    class="block text-gray-700 uppercase font-bold" 
                    for="propietario"
                >Nombre Propietario
                </label>
                <input
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
                    type="text"
                    placeholder="Nombre del propietario"
                    @input="$emit('update:propietario', $event.target.value)"
                    :value = "propietario"
                />
                <!-- :value = "propietario" (ej. v78) -->
                <!-- @input = "setPropietario" (ej. v78) -->
            </div>
            <div class="mb-5">
                <label
                    class="block text-gray-700 uppercase font-bold" 
                    for="email"
                >Email
                </label>
                <input
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
                    type="email"
                    id="email"
                    placeholder="correo@correo.com"
                    @input="$emit('update:email', $event.target.value)"
                    :value = "email"
                />
            </div>
            <div class="mb-5">
                <label
                    class="block text-gray-700 uppercase font-bold" 
                    for="alta"
                >Alta
                </label>
                <input
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
                    type="date"
                    id="alta"
                    @input="$emit('update:alta', $event.target.value)"
                    :value = "alta"
                />
            </div>
            <div class="mb-5">
                <label
                    class="block text-gray-700 uppercase font-bold" 
                    for="sintomas"
                >Síntomas
                </label>
                <textarea
                    class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40" 
                    id="sintomas"
                    placeholder="Describe los sintomas de este paciente"
                    @input="$emit('update:sintomas', $event.target.value)"
                    :value = "sintomas"
                />
            </div>
            <input 
                class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
                type="submit"
                value="Registrar Paciente"
            />
        </form>
    </div>
</template>
