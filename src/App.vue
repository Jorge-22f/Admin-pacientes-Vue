<script setup>
  import {ref, reactive, onMounted, watch} from 'vue';
  import {uid} from 'uid'
  import Header from './components/Header.vue';
  import Formulario from './components/Formulario.vue';
  import Paciente from './components/Paciente.vue';

  const pacientes = ref([])
  
  watch(pacientes, () => {
    guardarLocalStorage();
  }, {
    deep: true
  })

  onMounted(() => {
    const pacientesGuardados = localStorage.getItem('pacientes')
    if(pacientesGuardados){
      pacientes.value = JSON.parse(pacientesGuardados)
    }
  })

  const guardarLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

  const paciente = reactive({
    id: null,
    nombre: '',
    propietario: '',
    email: '',
    ingreso: '',
    sintomas: ''
  })

  const guardarPaciente = () => {
      if(paciente.id){
        const { id } = paciente
        const i = pacientes.value.findIndex(paciente => paciente.id === id)
        pacientes.value[i] = {...paciente}
      } else{
        pacientes.value.push({
        ...paciente,
        id: uid() // al crear una copia del objeto se quita la reactividad
        })
      }
    
    // Reiniciar formulario
    Object.assign(paciente, {
      nombre: '',
      propietario: '',
      email: '',
      ingreso: '',
      sintomas: '',
      id: null
    })
  }

  const actualizarPaciente = (id) => {
    const pacienreEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
    Object.assign(paciente, pacienreEditar)
  }

  const eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
  }

</script>

<template>
  <div class="container mx-auto mt-20"></div>
    <Header></Header>

    <div class="mt-12 md:flex">
      <Formulario 
        v-model:nombre="paciente.nombre"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:ingreso="paciente.ingreso"
        v-model:sintomas="paciente.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="paciente.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3x1 text-center">Pacientes</h3>

        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Informacion de
            <span class="text-indigo-600 font-bold">pacientes</span>
          </p>
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            @actualizar-paciente="actualizarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>
        <p v-else class="mt-20 text-2x1 text-center">No hay pacientes registrados</p>
      </div>
    </div>
</template>