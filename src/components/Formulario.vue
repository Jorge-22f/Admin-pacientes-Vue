<script setup>
    import {reactive, computed} from 'vue'
    import Alerta from './Alerta.vue'

    const alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:ingreso', 'update:sintomas', 'guardar-paciente'])

    const props = defineProps({
        id: { 
            type: [String, null],
            required: true
        },
        nombre: {
            type: String,
            required: true
        },
        propietario: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        ingreso: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        }
    })

    const validar = () => {
        if(Object.values(props).includes('')){
            alerta.mensaje = 'Todos los campos son obligatorios'
            alerta.tipo = 'error'
            return
        }
        emit('guardar-paciente')
        alerta.mensaje = 'Paciente almacenado correctamente'
        alerta.tipo = 'exito'

        setTimeout(() => {
            Object.assign(alerta, {
                tipo: '',
                mensaje: ''
            })
        }, 1500) 
    }

    const editando = computed(()=>{
        return props.id
    })
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3x1 text-center">Ingreso de pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
            Añadir pacientes y
            <span class="text-indigo-600 font-bold">Administralos</span>
        </p>

        <Alerta v-if="alerta.mensaje" :alerta="alerta"/>

        <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" v-on:submit.prevent="validar">
            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 uppercase font-bold">
                    Nombre mascota
                </label>
                <input id="mascota" type="text" placeholder="Nombre mascota" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                :value="nombre"
                @input="$emit('update:nombre', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">
                    Nombre del propietario
                </label>
                <input id="propietario" type="text" placeholder="Nombre propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                :value="propietario" @input="$emit('update:propietario', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">
                    Email del propietario
                </label>
                <input id="email" type="email" placeholder="Email Propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                :value="email" @input="$emit('update:email', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="ingreso" class="block text-gray-700 uppercase font-bold">
                    Fecha ingreso
                </label>
                <input id="ingreso" type="date" class="border-2 w-full p-2 mt-2 rounded-md"
                :value="ingreso" @input="$emit('update:ingreso', $event.target.value)">
            </div>
            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 uppercase font-bold">
                    Sintomas
                </label>
                <textarea id="sintomas" placeholder="Describe los sintomas" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                :value="sintomas" @input="$emit('update:sintomas', $event.target.value)"></textarea>                
            </div>
            <input type="submit" class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo cursor-pointer transition-colors" :value="[editando ? 'Guardar cambios' : 'Registrar paciente']"/>
        </form>
    </div>
</template>