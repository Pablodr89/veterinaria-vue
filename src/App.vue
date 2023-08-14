<script setup>
  import { ref, reactive, onMounted, watch } from 'vue'
  import { uid } from 'uid'
  import Header from './components/Header.vue'
  import Formulario from './components/Formulario.vue'
  import Paciente from './components/Paciente.vue'

  let pacientes = ref([])

  //Watch está pendiente a cualquier cambio, llama a la funcion para guardar los cambios en el storage y con el OnMounted hace persistente el arreglo de pacientes
  watch(pacientes, () => {
    pacienteLocalStorage()
  }, {deep: true})

  onMounted(() => {
    let pacientesStorage = localStorage.getItem('pacientes')
    if(pacientesStorage) {
      pacientes.value = JSON.parse(pacientesStorage)
    }
  })

  let pacienteLocalStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }
  //Colocamos el objeto de pacientes en el padre para poder comunicar mejor con los componentes hijos para poder modificar y mostrar de manera mas facil
  let paciente = reactive ({
      id: null,
      nombre: '',
      propietario: '',
      email: '',
      alta: '',
      sintomas: ''
    })

  let guardarPaciente = () => {
    if(paciente.id) {
      console.log('editando')
      let { id } = paciente
      let i = pacientes.value.findIndex(pacienteState => pacienteState.id === id)
      pacientes.value[i] = {...paciente}
    } else {
      pacientes.value.push({
        ...paciente, id: uid()
      })
    }

    Object.assign(paciente, {
      nombre: '',
      propietario: '',
      email: '',
      alta: '',
      sintomas: '',
      id: null
    })
  }

  let editarPaciente = (id) => {
    let pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
    Object.assign(paciente, pacienteEditar)
  }

  let eliminarPaciente = (id) => {
    pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
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
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            Información de
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>

          <Paciente 
            v-for="paciente in pacientes"
            :paciente="paciente"
            @editar-paciente="editarPaciente"
            @eliminar-paciente="eliminarPaciente"
          />
        </div>

        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
