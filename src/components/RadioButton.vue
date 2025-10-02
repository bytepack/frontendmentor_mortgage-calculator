<script setup>
const model = defineModel()
defineProps({
  label: {
    type: String,
    required: true,
  },
  name: {
    type: String,
    required: true,
  },
  value: {
    type: String,
    required: true,
  },
})
</script>
<template>
  <label class="group relative flex items-center gap-5 py-4 px-6 text-lg font-bold cursor-pointer">
    <input
      class="z-10 peer w-3 h-3 appearance-none cursor-pointer outline-2 outline-offset-2 outline-slate-400 rounded-full transition-colors duration-200 checked:bg-lime-500 checked:outline-lime-500 focus:outline-lime-800"
      type="radio"
      :name="name"
      :value="value"
      v-model="model"
    />
    <span
      class="absolute inset-0 rounded-lg border-2 border-slate-400 transition-colors duration-200 peer-checked:border-lime-500 peer-checked:bg-lime-100 group-hover:border-lime-500 peer-focus:border-lime-700"
    ></span>
    <span class="z-10">{{ label }}</span>
  </label>
</template>
