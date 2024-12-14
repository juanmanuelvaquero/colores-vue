<template>
    <form @submit.prevent="validarInput">
        <input type ="text" v-model="textoInput" placeholder ="rrr,ggg,bbb">
        <p class="error" v-bind:class="{ visible : error }">números entre 0 y 255 separados por comas</p>
        <input type="submit" value="crearcolor"> 
    </form>
</template>

<script setup>
    import {ref } from "vue"

    const textoInput = ref("")
    const error = ref(false)

    const emit = defineEmits(["crear"])

function validarInput(){

    error.value = false

    if(/^([0-9]{1,3},){2}[0-9]{1,3}$/.test(textoInput.value)){
                let valores =textoInput.value.split(",").map( n => Number(n))

                let valido = true

                let i = 0;

                // i++ es para los valores rgb. Con while hacemos una validación en cadena.
                while(valido && i < valores.length){
                    valido = valores[i] <= 255
                    i++
                }
                // cuando ponemos {} introducimos objetos
                
                if(valido){
                    let [r,g,b] = valores

                    return fetch("https://api-ejemplo-x5e3.onrender.com/colores/nuevo",{

                    method : "POST",
                    body : JSON.stringify({r,g,b}),
                    headers : {
                        "Content-type" : "application/json"
                    }
                })
                .then(respuesta => respuesta.json())
                .then(({id}) => {
                    
                    emit("crear", { id,r,g,b})
                    return textoInput.value = ""
                })



                }

            }
            error.value = true
}



//nota: cuando abrimos {} lo usamos para crear objetos
</script>