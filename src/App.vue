<script setup>
import { ref } from 'vue'
//importerar komponenter som används i appen
import KnappRad from './components/KnappRad.vue'
import ResultatRad from './components/ResultatRad.vue'
import PoangRad from './components/PoangRad.vue'

//skapar reaktiva variabler för spelets tillstånd
const resultat = ref({})
const knappar = ref([
  'sten',
  'sax',
  'pase',
  'eend',
  'art',
  'bendeen',
  'alice',
  'markiplier',
  'chicwich'
])
const vinnare = ref('')
const reset = ref(true)

//hanterar när knappar väljs och hittar vinnaren
function hittaVinnare(valdaKnappar) {
  vinnare.value = ''
  reset.value = false
  let spelare = knappar.value.indexOf(valdaKnappar.spelare)
  let dator = knappar.value.indexOf(valdaKnappar.dator)
  resultat.value = { spelare: spelare, dator: dator }
}

//uppdaterar poängställningen när en vinnare har hittats
function raknaPoang(v) {
  vinnare.value = v
}
</script>

<template>
  <header>
    <h1>Sten, sax, påse, coole eend, art, ben deen, patricia, markiplier and chicken sandwich!</h1>
  </header>

  <main>
    <KnappRad :knappar="knappar" @valda-knappar="hittaVinnare" :reset="reset" />
    <ResultatRad :valdaKnappar="resultat" @vinnare="raknaPoang" :reset="reset" />
    <PoangRad :vinnare="vinnare" :reset="reset" />
    <div class="score">
      <button id="nolla" @click="reset = true">Börja Om</button>
    </div>
  </main>
</template>

<style scoped>
button {
  padding: 0.6em 1.2em;
  font-size: 1.2em;
  color: white;
  background-color: #ff0000;
  border: 1px solid #000000cc;
  border-radius: 5px;
  cursor: pointer;
}

.knapprad {
  display: flex;
  justify-content: center;
  gap: 0.6em;
}

header {
  text-align: center;
  margin-bottom: 1.2em;
}

.resultat {
  font-size: 1.2em;
  text-align: center;
  margin: 1.2em 0;
}

.score {
  font-size: 1.2em;
  text-align: center;
}

#nolla {
  margin-top: 2em;
  padding: 1em 2em;
  font-size: 1em;
  border-radius: 100px;
  font-family: fantasy;
}

h1 {
  font-family: fantasy;
}
</style>
