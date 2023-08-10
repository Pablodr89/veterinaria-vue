<script setup>
    import { computed } from 'vue'
    let props = defineProps({
        alerta: {
            type: Object,
            required: true
        }
    })

    let isError = computed(() => {
        return props.alerta.tipo === 'error'
    })
</script>

<template>
    <div class="text-white text-center uppercase font-bold mb-3 rounded-md p-3" :class="[isError ? 'bg-red-500' : 'bg-green-500']">
        {{ alerta.mensaje }}
    </div>
</template>