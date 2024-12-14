<template>
    <li @click="emitir" v-bind:style=" { backgroundColor : `rgb(${[r,g,b].join(`,`)})` }">{{ r }},{{ g }},{{ b }}</li>
</template>

<script setup>
    const props = defineProps({
        id : Number,
        r : Number,
        g : Number,
        b : Number
    })

    const emit = defineEmits(["borrar"])

    function emitir(){

        fetch("https://api-ejemplo-x5e3.onrender.com/colores/borrar" + props.id, {
            method : "DELETE"
        })
        .then( respuesta => respuesta.json())
        .then( ({error}) => {
            if(!error){
                return emit ("borrar", props.id)
            }
        })
        //emit("borrar",props.id)
    }

</script>