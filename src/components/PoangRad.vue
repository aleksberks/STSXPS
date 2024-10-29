<script setup>
import { ref, watch } from 'vue'

//definierar props och score
const props = defineProps(['vinnare', 'reset'])
const score = ref({ spelare: 0, dator: 0 })

//övervakar props för att uppdatera poängen
watch(props, () => {
  if (props.vinnare == 'spelare') {
    score.value.spelare++
  } else if (props.vinnare === 'dator') {
    score.value.dator++
  }
})

//återställer poängen när reset-prop ändras
watch(
  () => props.reset,
  () => {
    if (props.reset) {
      score.value.spelare = 0
      score.value.dator = 0
    }
  }
)
</script>
<template>
  <div class="score">
    <p>
      <span id="spelare" class="spelare">
        {{ score.spelare }}
      </span>
      <span> - </span>
      <span id="dator" class="dator">
        {{ score.dator }}
      </span>
    </p>
  </div>
</template>
<style scoped>
.spelare {
  color: rgb(162, 255, 23);
}
.dator {
  color: rgb(3, 225, 225);
}
</style>
