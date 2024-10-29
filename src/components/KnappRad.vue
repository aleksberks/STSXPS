<script setup>
import { watch } from 'vue'
import artImg from '@/components/art.png'
import eendImg from '@/components/eend.png'
import paseImg from '@/components/pase.png'
import saxImg from '@/components/sax.png'
import stenImg from '@/components/sten.png'
import winSound from './win.mp3'
import loseSound from './lose.mp3'

// console.log test, ville se om websidan hittade vägen till bilderna (de gjorde den inte förut)
console.log('Image paths:', { artImg, eendImg, paseImg, saxImg, stenImg })

//kopplar ihop bildnamn med bildfiler
const imageMap = {
  art: artImg,
  eend: eendImg,
  pase: paseImg,
  sax: saxImg,
  sten: stenImg
}

//skapar ljudobjekt för win (spelare) och lose (dator)
const winAudio = new Audio(winSound)
const loseAudio = new Audio(loseSound)

//definierar props och emits
const props = defineProps(['knappar', 'reset'])
const emit = defineEmits(['valdaKnappar'])

//hanterar när spelaren gör sitt val
function spelarval(e) {
  //tar bort tidigare spelarval-markering
  let buttons = document.getElementsByClassName('alternativ')
  for (let b of buttons) {
    b.classList.remove('spelarval')
  }
  //markerar den valda knappen
  const clickedButton = e.target.closest('button')
  clickedButton.classList.add('spelarval')
  const datornsVal = datorval()

  //beräknar vem som vann och spela ljud
  const spelarIndex = props.knappar.indexOf(clickedButton.querySelector('img').alt)
  const datorIndex = props.knappar.indexOf(datornsVal)

  if (spelarIndex === datorIndex) {
    //oavgjort -> inget ljud
  } else if (
    (spelarIndex % 2 === datorIndex % 2 && spelarIndex > datorIndex) ||
    (spelarIndex % 2 !== datorIndex % 2 && spelarIndex < datorIndex)
  ) {
    winAudio.play() //winnar ljudet
  } else {
    loseAudio.play() //förlorar ljudet
  }

  // Skicka valen till förälderkomponenten
  emit('valdaKnappar', { spelare: clickedButton.querySelector('img').alt, dator: datornsVal })
}

// Hantera datorns val
function datorval() {
  // Slumpa fram datorns val
  let val = Math.floor(Math.random() * props.knappar.length)
  let buttons = document.getElementsByClassName('alternativ')
  // Ta bort tidigare datorval-markering
  for (let b of buttons) {
    b.classList.remove('datorval')
    b.title = ''
    // Markera datorns nya val
    if (b.querySelector('img').alt == props.knappar[val]) {
      b.classList.add('datorval')
      b.title = 'Datorns val'
    }
  }
  return props.knappar[val]
}

// Återställ spelet när reset-prop ändras
watch(
  () => props.reset,
  () => {
    if (props.reset) {
      // Ta bort alla markeringar
      let buttons = document.getElementsByClassName('alternativ')
      for (let b of buttons) {
        b.classList.remove('spelarval')
        b.classList.remove('datorval')
        b.title = ''
      }
    }
  }
)
</script>

<template>
  <div class="knapprad">
    <button v-for="knapp in props.knappar" class="alternativ" :key="knapp" @click="spelarval">
      <img :src="imageMap[knapp]" :alt="knapp" />
    </button>
  </div>
</template>

<style scoped>
button {
  padding: 1.2em 2.4em;
  font-size: 1.2em;
  background-color: #f0f0f0;
  border: 1px solid #cccc;
  border-radius: 100px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

img {
  width: 100px;
  height: 100px;
  object-fit: contain;
}

.knapprad {
  display: flex;
  justify-content: center;
  gap: 0.6em;
}

button.spelarval {
  background-color: greenyellow;
}

button.datorval {
  background-color: cyan;
}

button.spelarval.datorval {
  background-color: yellow;
}
</style>
