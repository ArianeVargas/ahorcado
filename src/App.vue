<template>
  <div class="container">
    <h1 class="titulo">Aho_cado</h1>
    <!-- Pantalla de selección de categoría -->
    <div v-if="!selectedCategory">
      <h2 class="seCat">Selecciona una Categoría</h2>
      <div class="category-select">
        <div class="category-card" v-for="category in categories" :key="category.value"
          @click="selectCategory(category.value)">
          <img :src="category.image" alt="Category Image" />
          <p class="catExs">{{ category.label }}</p>
        </div>
      </div>
    </div>
    <!-- Pantalla de selección de nivel -->
    <div v-else-if="!selectedLevel">
      <h2 class="selNiv">Selecciona un Nivel</h2>
      <div class="difficulty-buttons">
        <button class="btnNiv" @click="selectLevel('facil')">Fácil (10 intentos)</button>
        <button class="btnNiv" @click="selectLevel('medio')">Medio (7 intentos)</button>
        <button class="btnNiv" @click="selectLevel('dificil')">Difícil (3 intentos)</button>
      </div>
      <button class="inicio" @click="goToGame">Comenzar Juego</button>
    </div>
    <!-- Pantalla del juego -->
    <div v-else>
      <div>
        <div>
          <div class="caf">
            <p>{{ selectedCategory }}</p>
          </div>
          <div class="hangman-image">
            <!-- Mostrar el emoji del ahorcado -->
            <span>{{ hangmanImage }}</span>
          </div>
        </div>
        <div class="adLe">
          <p v-for="(letter, index) in word" :key="index" class="letter">
            {{ guessedLetters.includes(letter) ? letter : '_' }}
          </p>
        </div>
      </div>
      <div class="keyboard">
        <button v-for="letter in alphabet" :key="letter" @click="guessLetter(letter)"
          :disabled="guessedLetters.includes(letter) || isWinner || remainingAttempts === 0">
          {{ letter }}
        </button>
      </div>
      <div>
        <p class="intentos" v-if="remainingAttempts > 0 && !isWinner">Intentos restantes: {{ remainingAttempts }}</p>
        <p class="perdiste" v-else-if="isWinner">¡Ganaste!</p>
        <p class="perdiste" v-else>¡Perdiste! La palabra era: {{ selectedWord }}</p>

        <!-- Botones para volver a jugar o volver a la selección de nivel -->
        <div class="accion">
          <button @click="restartGame">Volver a Jugar</button>
          <button @click="returnToHome">Inicio</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';
import animales from "./assets/animales.jpeg";
import colores from "./assets/colores.jpeg";
import frutas from "./assets/frutas.jpeg";
import paises from "./assets/paises.jpeg";

const categoriesA = {
  animales: ['gato', 'perro', 'elefante', 'tigre'],
  frutas: ['manzana', 'banana', 'uva', 'sandía'],
  ciudades: ['paris', 'tokio', 'roma', 'londres'],
  colores: ['azul', 'amarillo', 'rojo', 'verde'],
};

const categories = [
  {
    label: 'Animales',
    value: 'animales',
    image: animales, // Reemplaza con la URL de la imagen de animales
  },
  {
    label: 'Frutas',
    value: 'frutas',
    image: frutas, // Reemplaza con la URL de la imagen de frutas
  },
  {
    label: 'Ciudades',
    value: 'ciudades',
    image: paises, // Reemplaza con la URL de la imagen de ciudades
  },
  {
    label: 'Colores',
    value: 'colores',
    image: colores, // Reemplaza con la URL de la imagen de colores
  },
];

const alphabet = 'abcdefghijklmnopqrstuvwxyz'.split('');
const selectedCategory = ref('');
const selectedLevel = ref('');
const selectedWord = ref('');
const guessedLetters = ref([]);
const remainingAttempts = ref(0);
const gameOver = ref(false);

const hangmanImages = {
  facil: ['😀', '😫', '😵', '😖', '😩', '😢', '😥', '😓', '😭', '💀'], // Nivel fácil con emojis de personaje muriendo
  medio: ['😀', '😫', '😵', '😩', '😢', '😓', '💀'],            // Nivel medio con emojis de personaje muriendo
  dificil: ['😀', '😵', '💀'],                                       // Nivel difícil con emojis de personaje muriendo
};
const hangmanImage = ref(''); // La imagen inicial, puedes cambiarla a tu gusto

const selectLevel = (level) => {
  selectedLevel.value = level;
  startGame(); // Iniciar el juego cuando se seleccione un nivel
};

const returnToHome = () => {
  selectedCategory.value = '';
  selectedLevel.value = '';
  selectedWord.value = '';
  guessedLetters.value = [];
  remainingAttempts.value = 0;
  gameOver.value = false;
};

const isWinner = computed(() => {
  // Verificamos que 'word' sea un array válido
  if (Array.isArray(word.value)) {
    return word.value.every(letter => guessedLetters.value.includes(letter));
  } else {
    return false; // En caso de que 'word' no sea un array válido
  }
});

const word = computed(() => {
  return selectedWord.value.split('');
});

const restartGame = () => {
  selectedLevel.value = '';
  guessedLetters.value = [];
  remainingAttempts.value = 0;
  gameOver.value = false;
  startGame(); // Inicia un nuevo juego con el mismo nivel
};

const selectCategory = (category) => {
  selectedCategory.value = category;
};

const startGame = () => {
  const category = categories.find(cat => cat.value === selectedCategory.value);
  if (category) {
    selectedWord.value = getRandomWord(category.value);
    guessedLetters.value = [];
    word.value = selectedWord.value ? selectedWord.value.split('') : [];
    if (selectedLevel.value === 'facil') {
      remainingAttempts.value = 10;
      hangmanImage.value = '😀'; // Emoji feliz para empezar
    } else if (selectedLevel.value === 'medio') {
      remainingAttempts.value = 7;
      hangmanImage.value = '😀'; // Emoji feliz para empezar
    } else if (selectedLevel.value === 'dificil') {
      remainingAttempts.value = 3;
      hangmanImage.value = '😀'; // Emoji feliz para empezar
    }
  }
};

const getRandomWord = (category) => {
  const categoryWords = categoriesA[category];
  if (Array.isArray(categoryWords) && categoryWords.length > 0) {
    const randomIndex = Math.floor(Math.random() * categoryWords.length);
    return categoryWords[randomIndex];
  } else {
    return ''; // Otra acción apropiada en caso de que no haya palabras en la categoría seleccionada
  }
};

const guessLetter = letter => {
  if (!gameOver.value && remainingAttempts.value > 0 && !guessedLetters.value.includes(letter)) {
    guessedLetters.value.push(letter);
    if (!selectedWord.value.includes(letter)) {
      remainingAttempts.value--;
      // Actualiza la imagen del ahorcado
      if (selectedLevel.value === 'facil') {
        hangmanImage.value = hangmanImages.facil[10 - remainingAttempts.value];
      } else if (selectedLevel.value === 'medio') {
        hangmanImage.value = hangmanImages.medio[7 - remainingAttempts.value];
      } else if (selectedLevel.value === 'dificil') {
        hangmanImage.value = hangmanImages.dificil[3 - remainingAttempts.value];
      }
    }
    checkGameStatus();
  }
};

const checkGameStatus = () => {
  const isComplete = word.value.every(letter => guessedLetters.value.includes(letter));
  if (isComplete) {
    isWinner.value = true; // Marca como ganado
    gameOver.value = true;
    return;
  }

  if (remainingAttempts.value === 0) {
    gameOver.value = true;
  }
};

watch(selectedCategory, selectCategory);
watch(selectedLevel, startGame);

</script>

<style scoped>
* {
  font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}

.titulo {
  color: rgba(255, 102, 0, 0.777);
  font-weight: 900;
  font-size: 5rem;
}

img {
  width: 95%;
  height: 75%;
  border-radius: 15px;
}

.container {
  margin: 3rem;
  text-align: center;
}

/* Estilos para la pantalla de selección de categoría */
/* Estilos para la pantalla de selección de categoría */
.category-select {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(18rem, 1fr));
  gap: 1.5rem;
  margin: 3em;
}

.category-card {
  height: 400px;
  padding: 1rem;
  background: #fff;
  border-radius: 0.9rem;
  border: 0.1rem solid rgb(0, 0, 0, 0.2);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  position: relative;
  overflow: hidden;
  text-align: center;
  border: 1px solid #1111;
}

.seCat {
  font-size: 3rem;
  font-style: italic;
  color: green;
}

.catExs {
  padding: .8rem;
  font-size: 2rem;
  color: white;
  background-color: rgba(255, 102, 0, 0.777);
  border-radius: 15px;
}

.catExs:hover {
  background-color: rgba(254, 166, 107, 0.667);
}

.selNiv {
  font-size: 3rem;
  font-style: italic;
  color: green;
}

.difficulty-buttons {
  display: flex;
  flex-direction: column;
  /* Coloca los botones en una columna */
  align-items: center;
  /* Centra los botones horizontalmente */
}

.difficulty-buttons button {
  width: 50%;
  /* Establece el ancho al 100% para que sean más largos */
  margin-bottom: 20px;
  /* Agrega espacio entre los botones */
}

.btnNiv {
  background: rgba(255, 102, 0, 0.777);
  color: #fff;
  padding: 1rem;
  border: none;
  border-radius: 15px;
  font-size: 2rem;
  font-weight: 700;
}

.caf p {
  font-size: 1rem;
  color: green;
  font-weight: 700;
  text-transform: uppercase;
}

.adLe {
  margin: 3rem;
}

.letter {
  display: inline-block;
  margin-right: 10px;
  /* Agrega espacio entre las letras */
}

/* Estilos para mostrar los botones como un teclado */
.keyboard {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  /* Define 7 columnas */
  gap: 10px;
  /* Espacio entre los botones */
  justify-content: center;
  /* Centra el contenido horizontalmente */
  align-content: center;
  /* Centra el contenido verticalmente */
  margin: 0 5rem;
}

.keyboard button {
  width: 100%;
  /* Ajusta el ancho de los botones al 100% de su contenedor */
  height: 50px;
  /* Ajusta la altura de los botones según tus necesidades */
  font-size: 20px;
  /* Tamaño de fuente de los botones */
  border: 1px solid #ccc;
  /* Agrega bordes a los botones */
  background-color: #fff;
  /* Fondo blanco para los botones */
  cursor: pointer;
  /* Cambia el cursor al pasar sobre los botones */
}

.keyboard button:hover {
  background-color: rgba(254, 166, 107, 0.667);
  /* Fondo blanco para los botones */
}

.intentos {
  font-size: 2rem;
  color: green;
  font-weight: 700;
  font-style: italic;
}

.perdiste {
  font-size: 2rem;
  color: green;
  font-weight: 700;
  font-style: italic;
}

.accion {
  margin: 2rem;
}

.accion button {
  background-color: #fff;
  border: 2px solid rgba(255, 102, 0, 0.777);
  padding: 1rem;
  border-radius: 15px;
  color: rgba(255, 102, 0, 0.777);
  font-size: 1.5rem;
  margin: 2rem;
}

.inicio {
  background-color: #fff;
  border: 2px solid rgba(255, 102, 0, 0.777);
  padding: 1rem;
  border-radius: 15px;
  color: rgba(255, 102, 0, 0.777);
  font-size: 1.5rem;
  margin: 2rem;
}

.hangman-image {
  font-size: 5rem;
}
</style>
