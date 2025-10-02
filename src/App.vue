<script setup>
import CalculatorSection from './components/CalculatorSection.vue'
import ResultSection from './components/ResultSection.vue'
import { reactive } from 'vue'

const result = reactive({ monthly: 0, total: 0, hasResult: false })

function saveResult(r) {
  result.monthly = r.monthly
  result.total = r.total
  result.hasResult = r.hasResult
}
</script>

<template>
  <main>
    <div
      class="bg-white md:grid md:grid-cols-2 md:rounded-3xl md:overflow-hidden lg:max-w-10/12 lg:mx-auto"
    >
      <CalculatorSection @calculate="saveResult" />
      <ResultSection :result="result" />
    </div>
  </main>
</template>
