<template>
  <LayoutApp>
    <template #header>
      <HeaderApp />
    </template>
    <template #resume>
      <ResumeApp :label="'Ahorro total'" :amount="amount" :totalAmount="totalAmount">
        <template #graphic>
          <GraphicResume :amounts="amounts" @select="select" />
        </template>
        <template #action> <ActionsApp @create="create" /> </template>
      </ResumeApp>
    </template>
    <template #movements>
      <MovementsApp :movements="movements" @remove="remove" />
    </template>
  </LayoutApp>
</template>

<script>
import LayoutApp from './LayoutApp.vue'
import HeaderApp from './HeaderApp.vue'
import ResumeApp from './ResumeApp/IndexResume.vue'
import MovementsApp from './MovementsApp/IndexApp.vue'
import ActionsApp from './ActionsApp.vue'
import GraphicResume from './ResumeApp/GraphicResume.vue'

export default {
  components: {
    LayoutApp,
    HeaderApp,
    ResumeApp,
    MovementsApp,
    ActionsApp,
    GraphicResume
  },
  data() {
    return {
      label: null,
      amount: null,
      movements: []
    }
  },
  computed: {
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date()
          const oldDate = today.setDate(today.getDate() - 30)

          return m.time > oldDate
        })
        .map((m) => m.amount)

      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i + 1)

        return lastMovements.reduce((suma, movement) => {
          return suma + movement
        }, 0)
      })
    },
    totalAmount() {
      return this.movements.reduce((suma, movement) => {
        return suma + movement.amount
      }, 0)
    }
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem('movements') || '[]')
    if (Array.isArray(movements)) {
      this.movements = movements?.map((m) => {
        return {
          ...m,
          time: new Date(m.time)
        }
      })
    }
  },
  methods: {
    create(movement) {
      this.movements.push({
        id: this.movements.length,
        ...movement
      })
      this.save()
    },
    remove(id) {
      this.movements = this.movements.filter((m) => m.id !== id)
      this.save()
    },
    save() {
      localStorage.setItem('movements', JSON.stringify(this.movements))
    },
    select(el) {
      console.log(el)
      this.amount = el
    }
  }
}
</script>

<style scoped>
/* Estilos específicos del componente */
</style>
