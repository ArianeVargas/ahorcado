<template>
  <div>
    <!-- niveles -->
    <div class="niveles">
      <div v-for="(nivelItem, index) in niveles" :key="index">
        <button class="Btn" @click="selectNivel(nivelItem)">{{ nivelItem.nombre }}</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from "vue";

const emit = defineEmits(['set-nivel'])

/* cambiar */
let niveles = [
  { nombre: "Facil", intentos: 10 },
  { nombre: "Medio", intentos: 7 },
  { nombre: "Dificil", intentos: 5 },
];

function selectNivel(n) {
  emit('set-nivel', n)
}

</script>
<style scoped></style>