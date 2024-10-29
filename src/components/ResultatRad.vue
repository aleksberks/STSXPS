<script setup>
import { ref, watch } from 'vue'

//definierar props och emits
const props = defineProps(['valdaKnappar', 'reset'])
const emit = defineEmits(['vinnare'])

//skapar en reaktiv variabel för att visa spelresultatet
const resultat = ref('Låt spelet börja!')

//övervakar ändringar i props för att uppdatera resultatet
watch(props, () => {
  //kontrollerar om det blev oavgjort
  if (props.valdaKnappar.spelare == props.valdaKnappar.dator) {
    resultat.value = 'Oavgjort!'
  }
  //kontrollerar om båda valen är jämna eller udda
  else if (props.valdaKnappar.spelare % 2 == props.valdaKnappar.dator % 2) {
    //om spelarens val är högre vinner spelaren
    if (props.valdaKnappar.spelare > props.valdaKnappar.dator) {
      resultat.value = 'Spelare vann!'
      emit('vinnare', 'spelare')
    }
    //annars vinner datorn
    else {
      resultat.value = 'Datorn vann!'
      emit('vinnare', 'dator')
    }
  }
  //när ett val är jämnt och ett är udda
  else {
    //om spelarens val är lägre vinner spelaren
    if (props.valdaKnappar.spelare < props.valdaKnappar.dator) {
      resultat.value = 'Spelare vann!'
      emit('vinnare', 'spelare')
    }
    //annars vinner datorn
    else {
      resultat.value = 'Datorn vann!'
      emit('vinnare', 'dator')
    }
  }
})

//återställer resultattexten när reset-prop ändras
watch(
  () => props.reset,
  () => {
    if (props.reset) {
      resultat.value = 'Låt spelet börja!'
    }
  }
)
</script>
<template>
  <div class="resultat">
    <p id="resultat">{{ resultat }}</p>
  </div>
</template>
<style scoped></style>
