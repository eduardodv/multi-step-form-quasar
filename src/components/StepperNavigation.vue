<template>
  <q-stepper-navigation class="tw-flex tw-justify-between">
    <div>
      <q-btn
        v-if="step > 1"
        flat
        :ripple="false"
        label="Go Back"
        class="tw-text-cool-gray tw-normal-case tw-text-base hover:tw-text-marine-blue tw-transition-all"
        @click="emit('handleClickPrevStep')"
      />
    </div>
    <q-btn
      :label="step === 4 ? 'Confirm' : 'Next Step'"
      class="tw-text-white tw-normal-case tw-text-base tw-px-5 tw-py-3 tw-min-w-[120px] tw-rounded-lg"
      :class="step === 4 ? 'tw-bg-purplish-blue' : 'tw-bg-marine-blue'"
      @click="emit('handleClickNextStep')"
      unelevated
    />
  </q-stepper-navigation>
</template>

<script setup lang="ts">
  defineProps({
    step: Number
  })

  const emit = defineEmits(['handleClickPrevStep', 'handleClickNextStep'])
</script>
