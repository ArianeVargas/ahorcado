<template>
  <div class="contenedorTo">
    <h1>Aho_cado</h1>

    <!-- categorias -->
    <Categorias v-if="!categoria" @set-category="setcategory" />

    <!-- niveles -->
    <Niveles v-if="categoria && !nivel " @set-nivel="setnivel" />

    <!-- jugar -->
    <Jugar v-if="nivel" :nivel="nivel" />

    <!-- volver a jugar -->
    <div class="reiniciarJuego">
      <button type="button" class="Btn" @click="clean()" v-if="resultado == 'gano' || resultado == 'perdio'"></button>
    </div>

  </div>
</template>

<script setup>
import { ref } from "vue"

import Jugar from './components/Jugar.vue'
import Niveles from './components/Niveles.vue'
import Categorias from "./components/Categorias.vue";

const categoria = ref('')
const nivel = ref('')/* esto es un objetoooooooooooooooooooooooooooooo!!!!!!!!!!!!!!!!!!!!!! */


const setcategory = (category) => {
  categoria.value = category
}
const setnivel = (nivel) => {
  nivel.value = nivel 
  console.log(nivel.value);
}


let key = ref();
let palabraGuiones = ref();
let palabra2 = ref("");
let resultado = ref("");



function selectColor() {
  nivel.value = null;
  palabra.value =
    colores.value[Math.floor(Math.random() * colores.value.length)];
  console.log(palabra.value);
  palabraGuiones.value = palabra.value.replace(/./g, "_ ");
  console.log(palabraGuiones.value);
}

function selectFruit() {
  nivel.value = null;
  palabra.value = frutas.value[Math.floor(Math.random() * frutas.value.length)];
  console.log(palabra.value);
  palabraGuiones.value = palabra.value.replace(/./g, "_ ");
  console.log(palabraGuiones.value);
}

function selectCountry() {
  nivel.value = null;
  palabra.value = paises.value[Math.floor(Math.random() * paises.value.length)];
  console.log(palabra.value);
  palabraGuiones.value = palabra.value.replace(/./g, "_ ");
  console.log(palabraGuiones.value);
}

function buscar(letra) {
  intentos.value++;
  key.value = letra;
  mostrar(letra);
  console.log(letra);
}

function mostrar(key) {
  let contador2 = 0;
  for (let i in palabra.value) {
    if (key == palabra.value[i]) {
      replacet(i * 2, key);
      contador2++;
      aciertos.value++;
    }
  }

  if (contador2 == 0) {
    errores.value++;
    if (errores.value == 1) {
      imagen.value = "./assets/1.png";
    } else if (errores.value == 2) {
      imagen.value = "./assets/2.png";
    } else if (errores.value == 3) {
      imagen.value = "./assets/3.png";
    } else if (errores.value == 4) {
      imagen.value = "./assets/4.png";
    } else if (errores.value == 5) {
      imagen.value = "./assets/5.png";
    } else if (errores.value == 6) {
      resultado.value = "perdio";
      imagen.value = "./assets/6.png";

      Swal.fire({
        title: "Has perdido",
        text: `Ya no te quedan más intentos`,
        icon: "error",
        confirmButtonText: "Ok",
        timer: 3000,
      });
    }
  }
  console.log(palabraGuiones.value);
}

function clean() {
  if (resultado.value == "gano" || resultado.value == "perdio") {
    palabra.value = "";
    key.value = "";
    palabraGuiones.value = "";
    intentos.value = 0;
    aciertos.value = 0;
    errores.value = 0;
    imagen.value = "";
    palabra2.value = "";
    resultado.value = "";
  }
}

function win() {
  palabra2 = palabra.value === palabraGuiones.value.replace(/ /g, "");
  console.log(palabra2);
  if (palabra.value === palabraGuiones.value.replace(/ /g, "")) {
    resultado.value = "gano";
    Swal.fire({
      title: "Has ganado",
      text: `Felicidades, lo hiciste bien`,
      icon: "success",
      confirmButtonText: "Ok",
      timer: 3000,
    });
  }
}
</script>

<style></style>