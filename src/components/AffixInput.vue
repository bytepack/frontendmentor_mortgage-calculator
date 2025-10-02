<script setup>
import { computed } from 'vue'

const model = defineModel()

const props = defineProps({
  label: {
    type: String,
    required: true,
  },
  error: {
    type: Boolean,
    default: false,
  },
  prefix: String,
  suffix: String,
  id: String,
})

const formattedValue = computed(() => {
  return model.value ? model.value.toLocaleString('en-US') : ''
})

function onInput(event) {
  let raw = event.target.value.replace(/[^0-9.]/g, '')

  const parts = raw.split('.')
  // allow only one dot
  if (parts.length > 2) {
    raw = parts[0] + '.' + parts.slice(1).join('')
  }

  model.value = raw === '' ? null : Number(raw)
  // force the field to show the formatted value only
  // (only when user typed a complete number but not 20.)
  if (parts[1] !== '') {
    event.target.value = raw ? Number(raw).toLocaleString('en-US') : ''
  }
}

function uniqueId() {
  return `affix-input-${Math.random().toString(30).substring(2)}`
}

const inputId = computed(() => props.id ?? uniqueId())
</script>

<template>
  <div class="min-w-0">
    <label class="text-slate-700" :for="inputId">{{ label }}</label>
    <div
      class="group flex mt-2 text-lg rounded-lg border-2 overflow-hidden transition-colors duration-200"
      :class="[error ? 'border-red-400' : 'border-slate-400 focus-within:border-lime-500']"
    >
      <span
        v-if="prefix"
        class="shrink-0 py-4 px-5 transition-colors duration-200"
        :class="[error ? 'bg-red-600 text-white' : 'bg-slate-300 group-focus-within:bg-lime-400']"
        >{{ prefix }}</span
      >
      <input
        :value="formattedValue"
        @input="onInput"
        class="min-w-15 flex-1 p-4 font-bold border-0 outline-none border-none"
        type="text"
        :id="inputId"
      />
      <span
        v-if="suffix"
        class="truncate py-4 px-5 transition-colors duration-200"
        :class="[error ? 'bg-red-600 text-white' : 'bg-slate-300 group-focus-within:bg-lime-400']"
        >{{ suffix }}</span
      >
    </div>
  </div>
</template>
