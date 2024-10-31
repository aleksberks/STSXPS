<script setup>
import { watch } from 'vue'

//tar bilderna som finns i components
import aliceImg from '@/components/images/alice.png'
import bendeenImg from '@/components/images/bendeen.png'
import artImg from '@/components/images/art.png'
import eendImg from '@/components/images/eend.png'
import paseImg from '@/components/images/pase.png'
import saxImg from '@/components/images/sax.png'
import stenImg from '@/components/images/sten.png'

//tar ljud filerna som finns i components
import winSound from './win.mp3'
import loseSound from './lose.mp3'

// console.log test, ville se om websidan hittade vägen till bilderna (de gjorde den inte förut)
console.log('Image paths:', { artImg, eendImg, paseImg, saxImg, stenImg })

//kopplar ihop bildnamn med bildfiler
const imageMap = {
  alice: aliceImg,
  bendeen: bendeenImg,
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
    // Vinnande logik: Spelare vinner om:
    (spelarIndex % 2 === datorIndex % 2 && spelarIndex > datorIndex) || //Båda valen är jämna/udda OCH spelarens val är högre
    (spelarIndex % 2 !== datorIndex % 2 && spelarIndex < datorIndex) //Ett val är jämnt och ett udda OCH spelarens val är lägre
  ) {
    winAudio.play() //winnar ljudet
  } else {
    loseAudio.play() //förlorar ljudet
  }

  //skickar valen till förälder komponenten (app.vue)
  emit('valdaKnappar', { spelare: clickedButton.querySelector('img').alt, dator: datornsVal })
}

//hanterar datorns val
function datorval() {
  //slumpar fram datorns val
  let val = Math.floor(Math.random() * props.knappar.length)
  let buttons = document.getElementsByClassName('alternativ')
  //tar bort tidigare datorval-markering
  for (let b of buttons) {
    b.classList.remove('datorval')
    b.title = ''
    //markerar datorns nya val
    if (b.querySelector('img').alt == props.knappar[val]) {
      b.classList.add('datorval')
      b.title = 'Datorns val'
    }
  }
  return props.knappar[val]
}

//återställer spelet när reset-prop ändras
watch(
  () => props.reset,
  () => {
    if (props.reset) {
      //tar bort alla markeringar
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
  padding: 1.3em 1.3em;
  font-size: 1.2em;
  background-color: #ffffff;
  border: 5px solid #ffffff;
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
  background-color: rgb(162, 255, 23);
}

button.datorval {
  background-color: rgb(12, 253, 253);
}

button.spelarval.datorval {
  background-color: rgb(234, 255, 0);
}
/* animationer för att knapparna ska wiggla */
@keyframes wiggle {
  0% {
    transform: rotate(0deg);
  }
  25% {
    transform: rotate(-20deg);
  }
  50% {
    transform: rotate(0deg);
  }
  75% {
    transform: rotate(20deg);
  }
  100% {
    transform: rotate(0deg);
  }
}

button:hover {
  animation: wiggle 0.45s ease-in-out infinite;
}
</style>
