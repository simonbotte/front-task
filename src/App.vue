<template>
  <div class="max-w-lg mx-auto py-20">
    <div class="flex items-center gap-2">
      <div class="bg-gray-300 rounded-full w-14 h-14"></div>
      <div>
        <label for="hours-old">{{ student.name }} is</label>
        <div class="flex gap-2">
          <InputNumber
            :value="student.age"
            @update:input-number="student.age = $event"
          />
          <div class="text">hours old</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import InputNumber from './components/InputNumber.vue'

const student = reactive<{
  name: string
  age: number | null
  image: string | null
}>({
  name: 'Hugh',
  age: null,
  image: null,
})
</script>

<style></style>
