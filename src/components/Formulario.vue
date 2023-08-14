<script setup>
    import { ref, reactive } from 'vue'
    import { computed } from 'vue';
    import Alerta from './Alerta.vue'

    let alerta = reactive({
        tipo: '',
        mensaje: ''
    })

    //Con ref, con v-model se crean 5 variables para los 5 inputs
    // let nombre = ref('')
    // let propietario = ref()
    // let email = ref()
    // let alta = ref()
    // let sintomas = ref()

    //Con reactive, con v-model creamos un objeto con todos los inputs del form
    // let paciente = reactive ({
    //     nombre: '',
    //     propietario: '',
    //     email: '',
    //     alta: '',
    //     sintomas: '',
    // })

    let validar = () => {
        if(Object.values(props).includes('')) {
            alerta.mensaje = 'Todos los campos son obligatorios'
            alerta.tipo = 'error'

            setTimeout(() => {
                alerta.mensaje = ''
                alerta.tipo = ''
            },3000)
            
            return
        } 

        emit('guardar-paciente')

        alerta.mensaje = 'Paciente almacenado con éxito'
        alerta.tipo = 'exito'
        
        setTimeout(() => {
            alerta.mensaje = ''
            alerta.tipo = ''
        },3000)
    }

    let emit = defineEmits(['update:nombre','update:propietario','update:email','update:alta','update:sintomas','guardar-paciente'])
    let props = defineProps({
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
        alta: {
            type: String,
            required: true
        },
        sintomas: {
            type: String,
            required: true
        },
    })

    let editando = computed(() => {
        return props.id
    })

    let eliminando = computed(() => {
        return props.id
    })
</script>

<template>
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento de pacientes</h2>

        <p class="text-lg mt-5 text-center mb-10">
            Añade pacientes
            <span class="text-indigo-600 font-bold">Adminístralos</span>
        </p>

        <Alerta v-if="alerta.mensaje" :alerta="alerta"/>
        <form action="" class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit.prevent="validar">
            <div class="mb-5">
                <label for="mascota" class="block text-gray-700 uppercase font-bold">
                    Nombre Mascota
                </label>
                <input type="text" class="border-2 w-full p-2 mt-2 rounded-md" id="mascota" placeholder="Nombre de la mascota" :value="nombre" @input="$emit('update:nombre', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="propietario" class="block text-gray-700 uppercase font-bold">
                    Nombre Propietario
                </label>
                <input type="text" class="border-2 w-full p-2 mt-2 rounded-md" id="propietario" placeholder="Nombre del propietario" :value="propietario" @input="$emit('update:propietario', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="email" class="block text-gray-700 uppercase font-bold">
                    Email
                </label>
                <input type="email" class="border-2 w-full p-2 mt-2 rounded-md" id="mascota" placeholder="Email del propietario" :value="email" @input="$emit('update:email', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="alta" class="block text-gray-700 uppercase font-bold">
                    Fecha Alta
                </label>
                <input type="date" class="border-2 w-full p-2 mt-2 rounded-md" id="alta" placeholder="Nombre de la mascota" :value="alta" @input="$emit('update:alta', $event.target.value)">
            </div>

            <div class="mb-5">
                <label for="sintomas" class="block text-gray-700 uppercase font-bold">
                    Sintomas
                </label>
                <textarea class="border-2 w-full p-2 mt-2 rounded-md h-40" id="sintomas" placeholder="Describe los sintomas de tu mascota" :value="sintomas" @input="$emit('update:sintomas', $event.target.value)"/>
            </div>

            <input type="submit" class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors" :value="[editando ? 'Editar Paciente' : 'Registrar Paciente']">
        </form>
    </div>
</template>