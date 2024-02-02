<template>
  <q-page :style-fn="boxHeight">
    <div class="tw-bg-white tw-shadow-md tw-rounded-xl tw-overflow-hidden">
      <q-stepper
        v-model="step"
        ref="stepper"
        class="tw-flex tw-p-4 tw-rounded-none"
        header-class="custom-steps"
      >
        <q-step
          :name="1"
          title="step 1"
          caption="Your info"
          :done="done1"
          color="transparent"
          active-icon="none"
        >
          <div class="tw-min-h-[480px] tw-flex tw-flex-col tw-justify-between">
            <div class="">
              step1
            </div>
            <StepperNavigation
              :step="step"
              @handleClickNextStep="clickNextStep($refs.stepper)"
              @handleClickPrevStep="clickPrevStep($refs.stepper)"
            />
          </div>
        </q-step>

        <q-step
          :name="2"
          title="step 2"
          caption="Select plan"
          :done="done2"
          color="transparent"
          active-icon="none"
        >
          <div class="tw-min-h-[480px] tw-flex tw-flex-col tw-justify-between">
            <div class="">
              step2
            </div>
            <StepperNavigation
              :step="step"
              @handleClickNextStep="clickNextStep($refs.stepper)"
              @handleClickPrevStep="clickPrevStep($refs.stepper)"
            />
          </div>
        </q-step>

        <q-step
          :name="3"
          title="step 3"
          caption="Add-ons"
          :done="done3"
          color="transparent"
          active-icon="none"
        >
          <div class="tw-min-h-[480px] tw-flex tw-flex-col tw-justify-between">
            <div class="">
              step3
            </div>
            <StepperNavigation
              :step="step"
              @handleClickNextStep="clickNextStep($refs.stepper)"
              @handleClickPrevStep="clickPrevStep($refs.stepper)"
            />
          </div>
        </q-step>

        <q-step
          :name="4"
          title="step 4"
          caption="Summary"
          :done="done2"
          color="transparent"
          active-icon="none"
        >
          <div class="tw-min-h-[480px] tw-flex tw-flex-col tw-justify-between">
            <div class="">
              step4
            </div>
            <StepperNavigation
              :step="step"
              @handleClickNextStep="clickNextStep($refs.stepper)"
              @handleClickPrevStep="clickPrevStep($refs.stepper)"
            />
          </div>
        </q-step>
      </q-stepper>
    </div>
  </q-page>
</template>

<script setup>
  import { ref } from 'vue';
  import StepperNavigation from 'src/components/StepperNavigation.vue';

  const step = ref(1)
  const done1 = ref(false)
  const done2 = ref(false)
  const done3 = ref(false)

  function boxHeight () {
    return { minHeight: 0 }
  }

  function clickPrevStep (prevRef) {
    prevRef.previous()
  }

  function clickNextStep (nextRef) {
    nextRef.next()
  }
</script>

<style lang="scss">
  .custom-steps {
    flex-direction: column;
    background: url('../assets/bg-sidebar-desktop.svg') left bottom no-repeat;
    background-size: cover;
    border-radius: 0.5rem;
    text-transform: uppercase;
    width: 260px;
    padding: 30px 10px;
    display: block;
    counter-reset: css-counter;

    & ~ div {
      flex: 1;
    }

    .q-stepper__dot::before,
    .q-stepper__label::after {
      display: none;
    }

    .q-stepper__dot {
      width: 33px;
      height: 33px;
      margin-right: 16px;
      margin-bottom: -5px;
      font-weight: bold;
      border: 1px solid #fff;

      span {
        i {
          display: none;
        }

        &::before {
          counter-increment: css-counter;
          content: counter(css-counter)
        }
      }
    }

    .q-stepper__tab {
      min-height: 0;
      margin-bottom: 14px;
      justify-content: flex-start;

      &:last-child {
        justify-content: flex-start;
      }

      &--active {
        .q-stepper__dot {
          border-color: hsl(206, 94%, 87%);
          background-color: hsl(206, 94%, 87%);

          span {
            color: hsl(213, 96%, 18%);
          }
        }
      }

      .q-stepper__title {
        font-size: 12px;
        line-height: 1.8;
        color: hsl(228, 100%, 84%);
      }

      .q-stepper__caption {
        color: #fff;
        font-size: 14px;
        font-weight: 500;
        letter-spacing: 1px;
      }
    }
  }

  .q-stepper__nav {
    .q-btn--flat {
      .q-focus-helper {
        display: none;
      }
    }
  }

  .q-stepper--horizontal .q-stepper__step-inner {
    padding: 1rem 5rem;
  }
</style>
