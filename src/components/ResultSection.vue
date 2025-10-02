<script setup>
defineProps({
  result: {
    type: Object,
  },
})
</script>
<template>
  <div class="grid place-items-center p-8 text-white bg-cyan-950 md:rounded-bl-[5rem]">
    <div v-if="result.hasResult" class="flex flex-col gap-4 text-white">
      <h2 class="text-2xl font-bold">Your result</h2>
      <p class="text-slate-300">
        Your result are shown below based on the information you provided. To adjust the result,
        edit the form and click “calculate repayments” again.
      </p>

      <div class="bg-lime-500 rounded-xl h-10 -mb-13"></div>
      <div class="flex flex-col gap-3 bg-[#021b26] rounded-lg p-6">
        <h3 class="text-lg text-slate-400">Your monthly repayments</h3>
        <p class="font-bold text-4xl text-lime-400">£{{ result.monthly }}</p>

        <hr class="border-slate-600 mt-2" />
        <h3 class="text-lg text-slate-400">Total you'll repay over the term</h3>
        <p class="font-bold text-lg">£{{ result.total }}</p>
      </div>
    </div>
    <div v-else class="flex flex-col items-center gap-4 text-center">
      <img width="190" height="190" src="@/assets/images/illustration-empty.svg" alt="No Result" />
      <h2 class="text-2xl">Result shown here</h2>
      <p>
        Complete the form and click “calculate repayments” to see what your monthly repayments would
        be.
      </p>
    </div>
  </div>
</template>
