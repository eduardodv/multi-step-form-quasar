<template>
  <div class="tw-mb-8">
    <h2 class="tw-text-marine-blue tw-font-bold tw-text-[32px]">{{ title }}</h2>
    <p class="tw-text-cool-gray tw-text-base">{{ subtitle }}</p>
  </div>
</template>

<script setup>
  defineProps({
    title: String,
    subtitle: String,
  })
</script>
