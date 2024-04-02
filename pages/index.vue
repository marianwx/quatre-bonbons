<template>
    <div class="container mx-auto p-4">
      <div class="text-center mb-4">
        <h1 class="text-3xl font-bold">Quatre BonBons</h1>
      </div>
  
      <div v-if="!gameOver">
        <div class="text-center mb-4">
          <h2 class="text-xl font-semibold">Încearcă să ghicești numărul!</h2>
        </div>
        <div class="flex justify-center items-center mb-4">
          <div v-if="generatedNumberVisible" class="flex space-x-2">
            <div v-for="(digit, index) in generatedNumber" :key="index" class="bg-gray-200 p-4 rounded-md text-center">
              {{ digit }}
            </div>
          </div>
          <button @click="toggleGeneratedNumber" class="ml-4 bg-green-500 text-white px-2 py-1 rounded-md"><svg width="30" height="30" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M10 22c-3.771 0-5.657 0-6.828-1.172C2 19.657 2 18.771 2 15M22 15c0 3.771 0 4.657-1.172 5.828C19.657 22 17.771 22 14 22M14 2c3.771 0 5.657 0 6.828 1.172C22 4.343 22 5.229 22 9M10 2C6.229 2 4.343 2 3.172 3.172 2 4.343 2 5.229 2 9" opacity=".5" stroke="#1C274C" stroke-linecap="round" stroke-width="1.5"/><path d="M5.892 14.06C5.297 13.37 5 13.025 5 12c0-1.025.297-1.37.892-2.06C7.08 8.562 9.072 7 12 7c2.927 0 4.92 1.562 6.108 2.94.595.69.892 1.035.892 2.06 0 1.025-.297 1.37-.892 2.06C16.92 15.438 14.928 17 12 17c-2.927 0-4.92-1.562-6.108-2.94Z" stroke="#1C274C" stroke-width="1.5"/><circle cx="12" cy="12" r="2" stroke="#1C274C" stroke-width="1.5"/></svg></button>
        </div>
        <div class="text-center mb-4">
          <h3 class="text-lg font-semibold">Introdu cifrele tale:</h3>
        </div>
        <div class="flex justify-center items-center space-x-4 mb-4">
          <div v-for="(input, index) in inputs" :key="index" class="bg-gray-200 p-4 rounded-md text-center">
            <input v-model="input.value" type="text" class="w-12 h-12 text-center" maxlength="1">
          </div>
        </div>
        <div class="text-center mb-4">
          <button @click="validate" :disabled="inputs.filter(input => input.value === '').length > 0" class="bg-blue-500 text-white px-4 py-2 rounded-md">Validează</button>
        </div>
        <div class="text-center mb-4" v-if="guessedPositions.length > 0">
          <p>Număr de poziții corecte ghicite: {{ guessedPositions }}</p>
        </div>
      </div>
  
      <div v-if="gameOver">
        <div class="text-center mb-4">
          <h2 class="text-xl font-semibold">Felicitări! Ai ghicit numărul!</h2>
          <p>Ai reușit să ghicești numărul {{ generatedNumber.join('') }} în {{ attempts }} încercări.</p>
        </div>
        <div class="text-center">
          <button @click="newGame" class="bg-blue-500 text-white px-4 py-2 rounded-md">Joc nou</button>
        </div>
      </div>
  
      <div class="text-center mt-8" v-if="guessHistory.length > 0">
        <h3 class="text-lg font-semibold mb-2">Istoricul ghicirilor tale:</h3>
        <ul>
          <li v-for="(historyItem, index) in guessHistory" :key="index">
            {{ historyItem.number }} - Poziții corecte: {{ historyItem.correctPositions }}
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        generatedNumber: [],
        generatedNumberVisible: false,
        inputs: [{ value: '' }, { value: '' }, { value: '' }, { value: '' }],
        guessedPositions: [],
        gameOver: false,
        attempts: 0,
        guessHistory: []
      }
    },
    methods: {
      generateNumber() {
        return Array.from({ length: 4 }, () => Math.floor(Math.random() * 10));
      },
      toggleGeneratedNumber() {
        this.generatedNumberVisible = !this.generatedNumberVisible;
        if (!this.generatedNumberVisible) {
          this.generatedNumber = [];
        } else {
          this.generatedNumber = this.generateNumber();
        }
      },
      validate() {
        const userGuess = this.inputs.map(input => parseInt(input.value));
        const correctPositions = this.generatedNumber.filter((digit, index) => digit === userGuess[index]).length;
        this.guessedPositions = correctPositions;
        if (correctPositions === 4) {
          this.gameOver = true;
        }
        this.attempts++;
        this.guessHistory.push({ number: userGuess.join(''), correctPositions });
        this.inputs.forEach(input => input.value = '');
      },
      newGame() {
        this.generatedNumberVisible = false;
        this.generatedNumber = [];
        this.inputs.forEach(input => input.value = '');
        this.guessedPositions = [];
        this.gameOver = false;
        this.attempts = 0;
        this.guessHistory = [];
      }
    },
    mounted() {
      this.generatedNumber = this.generateNumber();
    }
  }
  </script>
  
  <style scoped>
  /* Stilizare Tailwind CSS */
  </style>
  