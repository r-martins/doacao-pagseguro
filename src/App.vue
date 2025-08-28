<template>
  <form @submit.prevent="handleSubmit" class="mainForm" >
    <div class="logo">
      <img alt="Logo" :src="logo"/>
    </div>
    <h2 class="contrib-title">{{title}}</h2>
    <div class="frequency">
      <button class="unique" @click.prevent="frequency='U'" :class="{active: frequency == 'U'}">Única</button>
      <button class="monthly" @click.prevent="frequency='M'" :class="{active: frequency == 'M'}">Mensal</button>
      <p class="monthly-info" v-show="frequency == 'M'">Você pode cancelar a qualquer momento direto na sua conta PagSeguro.</p>
    </div>
    <hr/>
    <h2 class="amount-title">Escolha um valor</h2>
    <div class="amounts">
      <button class="option" @click.prevent="amount=10; useCustomAmount=false;" :class="{active: amount == 10}">R$ 10</button>
      <button class="option" @click.prevent="amount=50; useCustomAmount=false" :class="{active: amount == 50}">R$ 50</button>
      <button class="option" @click.prevent="amount=100; useCustomAmount=false" :class="{active: amount == 100}">R$ 100</button>
      <button class="option" @click.prevent="setUseCustom" :class="{active: useCustomAmount}">Outro Valor</button>
  <!--    <p>{{ amount }}</p>-->
      <hr/>
    </div>
    <div class="custom-amount" v-if="useCustomAmount">
      <label>Valor personalizado:</label>
      <CustomAmountInput  :amount="amount" ref="customAmountInput" @updateAmount="updateAmount"/>
    </div>
    <div class="error">{{errorMsg}}</div>
    <div v-if="!submitted">
      <button class="submit">Doar</button>
    </div>
    <div v-else>
      <div v-if="!payLink">
        <Spinner/>
      </div>
      <div v-show="payLink">
        <p class="payLink"><a :href="payLink" ref="aPayLink" target="_blank">Clique aqui</a> para prosseguir</p>
      </div>
    </div>
    <p class="by" v-if="!payLink">Contibuição feita via <a href="https://pbintegracoes.com/doar-com-pagseguro/?utm_source=doarbox" target="_blank">PagBank UOL</a></p>
  </form>
</template>

<script>

import {ref, watch} from "vue";
import CustomAmountInput from "@/components/CustomAmountInput";
import Spinner from "@/components/Spinner";

export default {
  name: 'App',
  components: {Spinner, CustomAmountInput},
  props: {
    receiverKey: {
      type: String,
      required: true
    }
  },
  setup() {
    const amount = ref(0.00) //final amount
    const useCustomAmount = ref(false)
    const customAmount = ref('')
    const customAmountInput = ref(null)
    const frequency = ref('U')
    const errorMsg = ref('')
    const submitted = ref(false)
    const payLink = ref(null)
    const aPayLink = ref(null)
    const title = ref('Contribua com nosso projeto')
    const logo = ref('https://cdn.jsdelivr.net/gh/r-martins/doacao-pagseguro@2/src/assets/pagseguro-contribua.png')
    

    function setUseCustom() {
      useCustomAmount.value = true;
      amount.value = 0;
      setTimeout(() => {
        // customAmountInput.value.setFocus()
      }, 1) //can't focus on hidden object, so I used a timeout
    }
    function handleSubmit() {
      submitted.value = true
      errorMsg.value = ''
      payLink.value = ''
      let formData = new FormData()
      formData.append('amount', amount.value)
      formData.append('frequency', frequency.value)
      formData.append('receiverEmail', document.getElementById('doar-pagseguro').dataset.receiveremail)
      
      fetch('https://ws.pbintegracoes.com/pspro/v7/donate/new', {
        method: 'POST',
        body: formData
      })
        .then(response => response.json())
        .then(data => {
          console.log(data)
          if (data.error) {
            throw Error(data.error.message + ' (err:' + data.error.code + ')')
          }
          if (data.redirectTo) {
            payLink.value = data.redirectTo
          }
        })
        .catch((err) => {
          console.log(err)
          submitted.value = false
          errorMsg.value = err.message
        })
      
    }
    
    function updateAmount(newAmount){
      amount.value = newAmount
    }
    
    watch(amount, () => {
      submitted.value = false
    })
    
    return {
      amount,
      useCustomAmount,
      customAmount,
      setUseCustom,
      customAmountInput,
      updateAmount,
      frequency,
      handleSubmit,
      errorMsg,
      submitted,
      payLink,
      aPayLink,
      logo,
      title
    }
  },
  beforeMount() {
    if (document.getElementById('doar-pagseguro').dataset.title) {
      this.title = document.getElementById('doar-pagseguro').dataset.title
    }
    if (document.getElementById('doar-pagseguro').dataset.logo) {
      this.logo = document.getElementById('doar-pagseguro').dataset.logo
    }
    
  }
}
</script>

<style scoped>
.mainForm {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 260px;
  background-color: #f9f9f9;
  padding: 10px;
  border: 1px solid #dedede;
}
.mainForm #doar-pagseguro hr {
  width: 70%;
  height:1px;
  border-width:0;
  color:#dedede;
  background-color:#dedede;
}
.mainForm #doar-pagseguro input {
  display: block;
  margin: 10px auto;
  width: 75%;
  box-sizing: border-box;
  padding: 10px;
  border: 1px solid #eee;
}
.mainForm #doar-pagseguro label {
  display: inline-block;
  margin-top: 10px;
  position: relative;
  font-size: 17px;
  margin-bottom: 10px;

}

.mainForm .logo {
  width: 230px;
  margin: 0 auto;
}
.mainForm .logo img{
  margin: 0 auto;
  width: 100%;
}

.mainForm .amounts .option, .mainForm .frequency button {
  color: #46a034;
  font-size: 16px;
  padding: 9px;
  font-family: Arial;
  font-weight: normal;
  border: 0;
  margin: 4px 0;
  cursor: pointer;
}
.mainForm .amounts .option{
  margin: 4px;
}
.mainForm .frequency button {
  font-size: 14px;
}
.mainForm h2 {
  font-size: 20px;
  font-weight: normal;
}
.mainForm .unique{
  border-top-left-radius: 3px;
  border-bottom-left-radius: 3px;
}
.mainForm .frequency button.monthly{
  border-top-right-radius: 3px;
  border-bottom-right-radius: 3px;
  background-color: rgba(197,197,204,0.28);
}

.mainForm .custom-amount {
  margin-top: 20px;
}

.mainForm .amounts button.option, .mainForm button.submit, .mainForm .frequency button {
  background-color: rgba(197,197,204,0.28);
}

.mainForm .option button.active, .mainForm .frequency button.active, .mainForm .amounts button.active, .mainForm button.submit, .mainForm .frequency button:hover, .mainForm .amounts button.option:hover, .mainForm button.submit:hover, .mainForm button.submit:active, .mainForm button.submit:focus{
  background-color: #2d7120;
  color: #fff;
}

.mainForm .error {
  color: red;
  font-size: 14px;
  margin: 14px 0;
  font-weight: bold;
}
.mainForm .by {
  font-size: 12px;
}
.mainForm .by a {
  text-decoration: none;
  color: inherit;
  font-weight: bold;
}
.mainForm .monthly-info {
  font-size: 14px;
}
.mainForm button.submit {
  padding: 12px 31px;
  background-color: #09a805;
  border: none;
  font-weight: bold;
  color: #fff;
  text-transform: uppercase;
  cursor: pointer;
}
.mainForm .payLink {
  font-weight: bold;
}
.mainForm .payLink a {
  color: #099b34;
  text-decoration-color: #c8c8c8;
}

.mainForm hr {
  width: 50%;
  height: 0px;
  border-top: 1px solid #d0cece;
  border-bottom: 0px;
}

.mainForm input.customAmountInput {
  font-size: 15px;
  line-height: 23px;
  width: 120px;
  border: 1px solid gray;
  color: #4b4a4a;
  font-family: unset;
  margin: auto;
}
</style>
