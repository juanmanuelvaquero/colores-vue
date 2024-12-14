<template>
    <formulario @crear ="crearColor" />
    <ul>
        <Item v-for="{id,r,g,b} in colores" :id="id" :r="r" :g="g" :b="b"  />
    </ul>
</template>

<script setup>
    import { ref,onMounted } from "vue"
    import Formulario from "./Formulario.vue"
    import Item from "./Item.vue"

    const colores =ref([])

    onMounted ( () => {
        fetch ("https://api-ejemplo-x5e3.onrender.com/colores")
        .then(respuesta => respuesta.json())
        .then(respuesta => {
            colores.value = respuesta
        })
    })

    function crearColor(color){
        colores.value.push(color)
    }

    //buscar info sobre push y filter. filter no cambia el valor de la función sino que lo filtra, push si lo cambia.
    function borrarColor(id){
        colores.value = colores.value.filter( color => color.id != id)
    }

</script>

