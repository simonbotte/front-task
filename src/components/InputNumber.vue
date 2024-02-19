<script setup lang="ts">
import { defineProps, defineEmits, ref, onMounted, watch } from 'vue'
const emit = defineEmits(['update:inputNumber'])
const props = defineProps<{
  value: number | null
}>()
const formattedValue = ref('')
const rawValue = ref(props.value ? props.value.toString() : '')

// Utilisez une fonction de surveillance pour réagir aux changements de la valeur des props
watch(
  () => props.value,
  (newValue) => {
    if (newValue === null) {
      formattedValue.value = ''
      rawValue.value = ''
    } else {
      formattedValue.value = formatValue(newValue)
      rawValue.value = newValue.toString()
    }
  }
)

const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement
  const cleanValue = target.value.replace(/[^\d\s]/g, '')
  formattedValue.value = formatValue(Number(cleanValue.replace(/\s/g, '')) || 0)
  rawValue.value = cleanValue.replace(/\s/g, '')

  const baseWidth = 72
  const widthPerCharacter = 10
  const newWidth = Math.max(baseWidth, cleanValue.length * widthPerCharacter)
  target.style.width = `${newWidth}px`

  emit('update:inputNumber', Number(rawValue.value) || 0)
}

const formatValue = (value: number | null) => {
  if (value === null) return ''
  return value.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ' ')
}

onMounted(() => {
  if (props.value !== null) {
    formattedValue.value = formatValue(props.value)
  }
})
</script>

<template>
  <input
    v-model="formattedValue"
    class="border-b-gray-300 border-b w-[72px]"
    type="text"
    @input="handleInput"
    ref="formattedValueInput"
  />
  <input v-model="rawValue" type="hidden" />
</template>
