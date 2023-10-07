<template>
    <div>
        <div class="categorias">
            <div class="cat">
                <h3>Animales</h3>
                <img src="../assets/animales.jpeg" alt="" id="ANIMALES" />
                <button class="Btn" type="button" value="animales" @click="categoriaSeleccionada('animal')">Jugar</button>
            </div>
            <div class="cat">
                <h3>Colores</h3>
                <img src="../assets/colores.jpeg" alt=" " id="COLORES" />
                <button class="Btn" type="button" value="colores" @click="categoriaSeleccionada('color')">Jugar</button>
            </div>
            <div class="cat">
                <h3>Frutas</h3>
                <img src="../assets/frutas.jpeg" alt=" " id="FRUTAS" />
                <button class="Btn" type="button" value="frutas" @click="categoriaSeleccionada('fruta')">Jugar</button>
            </div>
            <div class="cat">
                <h3>Paises</h3>
                <img id="PAISES" src="../assets/paises.jpeg" alt=" " />
                <button class="Btn" type="button" value="paises" @click="categoriaSeleccionada('pais')">Jugar</button>
            </div>
        </div>
    </div>
</template>

<script  setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['set-category'])

const categoriaSeleccionada = (categoria) => {
    emit('set-category', categoria)
}
</script>
<style scoped></style>