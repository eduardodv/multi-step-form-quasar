<template>
  <q-page :style-fn="boxHeight">
    content
  </q-page>
</template>

<script setup>
    function boxHeight () {
      return { minHeight: 0 }
    }
</script>
