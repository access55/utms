<script setup lang="ts">
defineProps<{
  options: string[]
}>()

const { modelValue } = defineModel<{
  modelValue: string
}>()
</script>

<template>
  <select
    v-model="modelValue"
    v-bind="$attrs"
    p="x-4 y-2"
    bg="transparent"
    max-w-full
    border="~ rounded gray-200 dark:gray-700"
    outline="none active:none"
    block
  >
    <option v-for="option in options" :key="option" :value="option">
      {{ option }}
    </option>
  </select>
</template>
