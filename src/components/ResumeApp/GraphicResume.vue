<template>
  <div>
    <svg @touchstart="tap" @touchmove="tap" @touchend="untap" viewBox="0 0 300 200">
      <line stroke="#c4c4c4" stroke-width="2" x1="0" :y1="zero" x2="300" :y2="zero" />
      <polyline fill="none" stroke="#0689B0" stroke-width="2" :points="points" />
      <line
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="200"
      />
    </svg>
    <p>Últimos 30 días</p>
  </div>
</template>

<script setup>
import { ref, toRefs, defineProps, computed, watch, defineEmits } from 'vue'

const props = defineProps({
  amounts: {
    type: Array,
    default: () => []
  }
})

const { amounts } = toRefs(props)

const amountToPixels = (amount) => {
  const min = Math.min(...amounts.value)
  const max = Math.max(...amounts.value)

  const amountAbs = amount + Math.abs(min)
  const minmax = Math.abs(max) + Math.abs(min)

  return 200 - ((amountAbs * 100) / minmax) * 2
}

const zero = computed(() => {
  return amountToPixels(0)
})

const points = computed(() => {
  const total = amounts.value.length
  return amounts.value.reduce(
    (points, amount, i) => {
      const x = (300 / total) * (i + 1)
      const y = amountToPixels(amount)
      return `${points} ${x},${y}`
    },
    `0, ${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`
  )
})

const showPointer = ref(false)
const pointer = ref(0)

const emit = defineEmits(['select'])

watch(pointer, (value) => {
  const index = Math.ceil(value / (300 / amounts.value.length))
  if (index < 0 || index > amounts.value.length) return
  emit('select', amounts.value[index - 1])
})

const tap = ({ target, touches }) => {
  showPointer.value = true
  const elementWidth = target.getBoundingClientRect().width
  const elementX = target.getBoundingClientRect().x
  const touchX = touches[0].clientX
  pointer.value = ((touchX - elementX) * 300) / elementWidth
}

const untap = () => {
  showPointer.value = false
}
</script>

<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>

<!-- amounts(){
//last days es igual a todo el array de movements
            const lastDays = this.movements
//filtramos cada movement
            .filter(m =>{
//la fecha de hoy
                const today = new Date();
//a la fecha de hoy le restamos 30 días
                const oldDate = today.setDate(today.getDate() - 30);
//ahora comparamos esas fechas, la fecha del movement debe ser mayor a la oldDay que es la fecha de hace 30 días
                return m.time > oldDate;
            })
//ahora por cada fecha que este dentro de los 30 días, tomamos su amount
            .map(m => m.amount)

//ahora retorno lastDays, lastDays es igual a todos los amount de los movements de no más de 30 días
            return lastDays.map((m, i) => {
//toma el desde el primer elemento (0) hasta el elemento anterior al acutal (i)
                const lastMovements = lastDays.slice(0,i);
//suma todos los movimientos menos el más reciente puesto que así lo decidimos el el slice()
                return lastMovements.reduce((suma, movement) => {
                    return suma + movement;
//el valor inicial es 0
                },0)
            });
        } -->
