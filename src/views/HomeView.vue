<script setup>
import { ref } from "vue";

// reactive state
const text = ref("PERU");
const number = ref("3");
const result = ref("");

function createMAp(alphabets, shift) {
  return alphabets.reduce((charsMap, currentChar, charIndex) => {
    const copy = { ...charsMap };
    let ind = (charIndex + shift) % alphabets.length;
    if (ind < 0) {
      ind += alphabets.length;
    };
    copy[currentChar] = alphabets[ind];
    return copy;
  }, {});
}

function encrypt() {
  const alphabetsLower = "abcdefghijklmnopqrstuvwxyz".split("");
  const alphabetsUpper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("");
  const mapLower = createMAp(alphabetsLower, parseInt(number.value));
  const mapUpper = createMAp(alphabetsUpper, parseInt(number.value));

  result.value = text.value
    .split("")
    .map((char) => {
      if (char === char.toUpperCase()) {
        return mapUpper[char];
      }
      if (char === char.toLowerCase()) {
        return mapLower[char];
      }
    })
    .join("");
}
</script>

<template>
  <div class="cesar">
    <h2>Algoritmo Cesar</h2>
    <form action="">
      <div class="form-group mb-2">
        <input
          type="text"
          class="form-control"
          placeholder="Texto"
          v-model="text"
        />
      </div>

      <div class="form-group mb-2">
        <input
          type="text"
          class="form-control"
          placeholder="Número"
          v-model="number"
        />
      </div>


      <div class="form-group mb-2">
        <input
          type="text"
          class="form-control"
          placeholder="Resultado"
          v-model="result"
        />
      </div>

      <button type="button" class="btn btn-success mb-2" @click="encrypt">Generar</button>
    </form>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .cesar {
    min-height: 100vh;
    align-items: center;
  }
  .cesar form {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
}
@media (max-width: 1023px) {
  .cesar button{
    width: 100%;
  }
}
</style>
