<template>
  <input type="text" v-model="customAmount" @blur="formatAndUpdateAmount" @focus="removeSymbol"
         ref="customAmountInput" placeholder="R$ 123,99"/>
</template>

<script>
import {ref} from "vue";

export default {
  name: "CustomAmountInput",
  props: ['amount'],
  setup(props, {emit}) {
    const customAmount = ref('')
    const customAmountInput = ref(null)
    const formattedAmountWithSymbol = ref('')
    

    const formatAndUpdateAmount = () => {
      let newAmount = parseFloat(customAmount.value.replace(/[^0-9,\.]+/, '').replace('.', '').replace(',', '.') * 1)
      emit('updateAmount', newAmount)
      formattedAmountWithSymbol.value = newAmount.toLocaleString('pt-br', {style: 'currency', currency: 'BRL'})
      customAmount.value = formattedAmountWithSymbol.value
    }

    const removeSymbol = () => {
      customAmount.value = customAmount.value.replace(/[^0-9,\.]+/, '')
    }
    
    
    return {customAmount,customAmountInput, formattedAmountWithSymbol, removeSymbol, formatAndUpdateAmount}
  },
  mounted() {
    this.$nextTick(function () {
      this.$el.focus()
    })
  }
}
</script>

<style scoped>

</style>