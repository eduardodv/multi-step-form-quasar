<template>
  <q-stepper-navigation class="tw-flex tw-justify-between">
    <div>
      <q-btn
        v-if="step > 1"
        flat
        :ripple="false"
        label="Go Back"
        class="tw-text-cool-gray tw-normal-case tw-text-base hover:tw-text-marine-blue tw-transition-all"
        @click="handleClickPrevStep"
      />
    </div>
    <q-btn
      :label="step === 4 ? 'Confirm' : 'Next Step'"
      class="tw-text-white tw-normal-case tw-text-base tw-px-5 tw-py-3 tw-min-w-[120px] tw-rounded-lg"
      :class="step === 4 ? 'tw-bg-purplish-blue' : 'tw-bg-marine-blue'"
      @click="handleClickNextStep"
      unelevated
    />
  </q-stepper-navigation>
</template>

<script setup>
  import { defineEmits } from 'vue'

  defineProps({
    step: Number
  })

  const emit = defineEmits(['handleClickPrevStep, handleClickNextStep'])

  function handleClickPrevStep () {
    emit('handleClickPrevStep')
  }

  function handleClickNextStep () {
    emit('handleClickNextStep')
  }
</script>
