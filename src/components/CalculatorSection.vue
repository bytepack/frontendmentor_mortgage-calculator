<script setup>
import AffixInput from '@/components/AffixInput.vue'
import RadioButton from '@/components/RadioButton.vue'
import { ref, reactive } from 'vue'

const emit = defineEmits(['calculate'])

const amount = ref(null)
const term = ref(null)
const rate = ref(null)
const mortgageType = ref('')

const errors = reactive({ amount: '', term: '', rate: '', mortgageType: '' })

const result = reactive({ monthly: '', total: '', hasResult: false })

function calculate() {
  if (!validateForm()) {
    result.hasResult = false
    emit('calculate', result)
    return
  }

  let monthly = 0
  let total = 0
  const monthlyRate = rate.value / 100 / 12
  const termInMonth = term.value * 12

  if (mortgageType.value === 'repayment') {
    const growthFactor = Math.pow(1 + monthlyRate, termInMonth)
    monthly = (amount.value * (monthlyRate * growthFactor)) / (growthFactor - 1)
  } else {
    monthly = amount.value * monthlyRate
  }

  total = monthly * termInMonth
  result.monthly = Number(monthly.toFixed(2)).toLocaleString()
  result.total = Number(total.toFixed(2)).toLocaleString()
  result.hasResult = true
  emit('calculate', result)
}

function validateAmount() {
  errors.amount = amount.value === null ? 'This field is required' : ''
}

function validateTerm() {
  errors.term = term.value === null ? 'This field is required' : ''
}

function validateRate() {
  errors.rate = rate.value === null ? 'This field is required' : ''
}

function validateMortgageType() {
  errors.mortgageType = mortgageType.value === '' ? 'This field is required' : ''
}
function validateForm() {
  validateAmount()
  validateTerm()
  validateRate()
  validateMortgageType()
  return !errors.amount && !errors.term && !errors.rate && !errors.mortgageType
}

function clearAll() {
  amount.value = null
  term.value = null
  rate.value = null
  mortgageType.value = ''
}
</script>
<template>
  <div class="bg-white p-8 capitalize">
    <div class="flex flex-col gap-1.5 md:flex-row md:justify-between">
      <h1 class="text-slate-800 text-xl font-bold">mortgage calculator</h1>
      <button
        @click="clearAll"
        class="capitalize text-lg text-slate-700 underline cursor-pointer focus:outline-2 focus:outline-offset-4 focus:outline-lime-500"
      >
        clear all
      </button>
    </div>
    <form @submit.prevent="calculate" class="flex flex-col gap-6 mt-6">
      <div class="space-y-2">
        <AffixInput
          :error="Boolean(errors.amount)"
          v-model="amount"
          label="mortgage amount"
          type="text"
          prefix="£"
        />
        <p v-if="errors.amount" class="text-red-600">{{ errors.amount }}</p>
      </div>

      <div class="grid w-full gap-6 md:grid-cols-2">
        <div class="space-y-2">
          <AffixInput
            :error="Boolean(errors.term)"
            v-model="term"
            label="mortgage term"
            suffix="years"
          />
          <p v-if="errors.term" class="text-red-600">{{ errors.term }}</p>
        </div>

        <div class="space-y-2">
          <AffixInput
            :error="Boolean(errors.rate)"
            v-model="rate"
            label="interest rate"
            suffix="%"
          />
          <p v-if="errors.rate" class="text-red-600">{{ errors.rate }}</p>
        </div>
      </div>

      <div class="flex flex-col gap-2">
        <p class="text-slate-700">mortgage type</p>
        <RadioButton
          value="repayment"
          v-model="mortgageType"
          name="mortgage-type"
          label="repayment"
        />
        <RadioButton
          value="interest-only"
          v-model="mortgageType"
          name="mortgage-type"
          label="interest only"
        />
        <p v-if="errors.mortgageType" class="text-red-600">{{ errors.mortgageType }}</p>
      </div>

      <button
        class="flex justify-center gap-4 text-lg font-bold px-5 py-4 rounded-full cursor-pointer capitalize transition-colors duration-200 bg-lime-500 hover:bg-lime-400 focus:outline-lime-900"
        type="submit"
      >
        <img width="24" height="24" src="@/assets/images/icon-calculator.svg" alt="calculator" />
        calculate repayments
      </button>
    </form>
  </div>
</template>
