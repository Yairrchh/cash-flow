<template>
  <div class="movement">
    <div class="content">
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img src="../../assets/tras-icon.svg" alt="borrar" @click="remove" />
      <p :class="{ red: isNegative, green: !isNegative }">{{ amoutCurrency }}</p>
    </div>
  </div>
</template>

<script setup>
import { toRefs, defineProps, computed, defineEmits } from 'vue'

const amoutCurrency = computed(() => {
  return currencyFormatter.format(amount.value)
})

const currencyFormatter = new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'USD'
})

const props = defineProps({
  id: {
    type: Number
  },
  title: {
    type: String
  },
  description: {
    type: String
  },
  amount: {
    type: Number
  }
})

const { id, title, description, amount } = toRefs(props)

const emit = defineEmits(['remove'])

const remove = () => {
  emit('remove', id.value)
}

const isNegative = computed(() => {
  return amount.value < 0
})

// export default {
//   props: {
//     id: {
//       type: Number
//     },
//     title: {
//       type: String
//     },
//     description: {
//       type: String
//     },
//     amount: {
//       type: Number
//     }
//   },

//   computed: {
//     amoutCurrency() {
//       return new Intl.NumberFormat('en-US', {
//         style: 'currency',
//         currency: 'USD'
//       }).format(this.amount)
//     },
//     isNegative() {
//       return this.amount < 0
//     }
//   },

//   methods: {
//     remove() {
//       this.$emit('remove', this.id)
//     }
//   }
// }
</script>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #a7c3cc80;
  border-radius: 8px;
  box-sizing: border-box;
}

.movement .content {
  width: 100%;
}

.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}

h4,
p {
  margin: 0;
  padding: 0;
}

h4 {
  margin-bottom: 8px;
}

.movement .action img {
  margin-bottom: 16px;
}

.red {
  color: red;
}

.green {
  color: green;
}
</style>
