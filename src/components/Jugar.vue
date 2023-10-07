<template>
    <div>
        <!-- botones -->
        <div class="estados" v-show="palabra != ''">
            <div class="int">
                <h4>Intentos</h4>
            </div>
            <div class="int">
                <h4>Aciertos</h4>
            </div>
            <div class="int">
                <h4>Errores</h4>
            </div>

            <div class="mostrarEstado" v-show="palabra != ''">
                <div><button></button>{{ intentos }}</div>
                <div><button></button>{{ aciertos }}</div>
                <div><button></button>{{ errores }}</div>
            </div>
        </div>

        <!-- imagen -->
        <div class="imagenAhorcado" v-show="palabra !== '' && nivel !== null">
            <img :src="imagen" alt="Avance del Ahorcado" />
        </div>

        <!-- guiones sobre los que se escribe -->
        <div class="guiones">
            <h1>{{ palabraGuiones }}</h1>
        </div>

        <!-- teclado para jugar -->
        <div class="err" v-show="errores <= 5">
            <div class="res" v-show="resultado == ''">
                <div class="pal" v-show="palabra != ''">
                    <button class="teclado" @click="buscar(letter)" v-for="letter in letras" :key="letter">
                        <p>{{ letter }}</p>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'

let palabra = ref("");

let intentos = ref(0);
let aciertos = ref(0);
let errores = ref(0);
let imagen = ref('');

let letras = ref([
    "A",
    "B",
    "C",
    "D",
    "E",
    "F",
    "G",
    "H",
    "I",
    "J",
    "K",
    "L",
    "M",
    "N",
    "Ñ",
    "O",
    "P",
    "Q",
    "R",
    "S",
    "T",
    "U",
    "V",
    "W",
    "X",
    "Y",
    "Z",
]);
let animales = ref(["VACA", "PERRO", "GATO", "CONEJO", "PAJARO", "LORO"]);
let colores = ref(["AZUL", "MORADO", "AMARILLO", "ROJO", "VERDE", "NEGRO"]);
let paises = ref([
    "COLOMBIA",
    "RUSIA",
    "ESPAÑA",
    "ECUADOR",
    "BRAZIL",
    "FRANCIA",
]);
let frutas = ref(["HIGO", "LIMA", "MANDARINA", "MANZANA", "MELON", "FRESA"]);
let imagenesAhorcado = [
    "./assets/1.png",
    "./assets/2.png",
    "./assets/3.png",
    "./assets/4.png",
    "./assets/5.png",
    "./assets/6.png"
];

onMounted(() => {
    seleccionarPalabra() 
})

const seleccionarPalabra = () => {
    console.log('jbjb');
    /* nivel.value = null;
    palabra.value =
      animales.value[Math.floor(Math.random() * animales.value.length)];
    console.log(palabra.value);
    palabraGuiones.value = palabra.value.replace(/./g, "_ ");
    console.log(palabraGuiones.value); */
}

</script>
<style scoped></style>