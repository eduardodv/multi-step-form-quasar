<template>
  <q-page :style-fn="boxHeight">
    <div class="tw-bg-white tw-shadow-md tw-rounded-xl tw-overflow-hidden">
      <q-form
        @submit="submitForm"
        greedy
        ref="formRef"
      >
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
            :done="step > 1"
            color="transparent"
            active-icon="none"
          >
            <div class="tw-min-h-[520px] tw-flex tw-flex-col tw-justify-between">
              <div class="tw-pl-4 tw-py-3">
                <HeaderForm
                  title="Personal info"
                  subtitle="Please provide your name, email address and phone number."
                />
                <div class="form-input">
                  <span class="tw-mb-1 tw-block tw-text-marine-blue">Name</span>
                  <q-input
                    ref="step1RefName"
                    v-model="form.name"
                    placeholder="e.g. Stephen King"
                    outlined
                    input-class="tw-font-medium tw-text-base tw-text-marine-blue placeholder:tw-text-cool-gray placeholder:tw-opacity-100"
                    :rules="[
                      val => val && val.length > 0 || 'This field is required',
                    ]"
                  />
                  <span class="tw-mb-1 tw-block tw-text-marine-blue">E-mail address</span>
                  <q-input
                    ref="step1RefEmail"
                    v-model="form.email"
                    placeholder="e.g. stephenking@lorem.com"
                    outlined
                    input-class="tw-font-medium tw-text-base tw-text-marine-blue placeholder:tw-text-cool-gray placeholder:tw-opacity-100"
                    :rules="[
                      val => val && val.length > 0 || 'This field is required',
                      val => val && validateEmail(val) || 'Please enter a valid email address'
                    ]"
                  />
                  <span class="tw-mb-1 tw-block tw-text-marine-blue">Phone Number</span>
                  <q-input
                    ref="step1RefPhone"
                    v-model="form.phone"
                    placeholder="e.g. (12) 3456-7890"
                    outlined
                    input-class="tw-font-medium tw-text-base tw-text-marine-blue placeholder:tw-text-cool-gray placeholder:tw-opacity-100"
                    mask="(##) ####-#####"
                    unmasked-value
                    :rules="[
                      val => val && val.length > 0 || 'This field is required',
                      val => val && val.length > 9 || 'Please enter a valid phone number'
                    ]"
                  />
                </div>
              </div>
              <StepperNavigation
                :step="step"
                @handleClickNextStep="clickNextStep"
                @handleClickPrevStep="clickPrevStep"
              />
            </div>
          </q-step>

          <q-step
            :name="2"
            title="step 2"
            caption="Select plan"
            :done="step > 2"
            color="transparent"
            active-icon="none"
          >
            <div class="tw-min-h-[520px] tw-flex tw-flex-col tw-justify-between">
              <div class="tw-pl-4 tw-py-3">
                <HeaderForm
                  title="Select your plan"
                  subtitle="You have the option of monthly or yearly billing."
                />
                <div class="tw-grid tw-grid-cols-3 tw-gap-5">
                  <label
                    v-for="(plan, index) in plansOptions"
                    :key="index"
                    class="tw-relative"
                  >
                    <q-radio
                      v-model="form.plan"
                      :val="plan"
                      :name="plan.value"
                      class="step-plan tw-absolute tw-opacity-0 tw-mw-1 tw-mh-1"
                    />
                      <div
                        class="block tw-rounded-md tw-cursor-pointer tw-border tw-p-4 tw-border-light-gray hover:tw-border-purplish-blue focus:tw-border-purplish-blue tw-transition-all"
                      >
                      <q-icon
                        :name="getPlanIcon(plan.value)"
                        size="40px"
                      />
                      <h3 class="tw-text-marine-blue tw-mt-10 tw-font-medium tw-text-base">
                        {{ plan.label }}
                      </h3>
                      <span class="tw-text-cool-gray tw-block">
                        ${{ form.plan_yearly ? `${plan.yearly}/yr` : `${plan.monthly}/mo`}}
                      </span>
                      <span v-if="form.plan_yearly" class="tw-text-marine-blue tw-text-xs">
                        {{ plan.label_promo }}
                      </span>
                    </div>
                  </label>
                </div>
                <div class="tw-w-full tw-gap-3 tw-flex tw-items-center tw-justify-center tw-bg-magnolia tw-mt-9 tw-rounded-md tw-p-1">
                  <span
                    class="tw-font-medium"
                    :class="!!form.plan_yearly ? 'tw-text-cool-gray' : 'w-text-marine-blue'"
                  >
                    Monthly
                  </span>
                  <q-toggle
                    v-model="form.plan_yearly"
                    class="custom-toggle"
                  />
                  <span
                    class="tw-font-medium"
                    :class="form.plan_yearly ? 'tw-text-marine-blue' : 'tw-text-cool-gray'"
                  >
                    Yearly
                  </span>
                </div>
              </div>
              <StepperNavigation
                :step="step"
                @handleClickNextStep="clickNextStep"
                @handleClickPrevStep="clickPrevStep"
              />
            </div>
          </q-step>

          <q-step
            :name="3"
            title="step 3"
            caption="Add-ons"
            :done="step > 3"
            color="transparent"
            active-icon="none"
          >
            <div class="tw-min-h-[520px] tw-flex tw-flex-col tw-justify-between">
              <div class="tw-pl-4 tw-py-3">
                <HeaderForm
                  title="Pick add-ons"
                  subtitle="Add-ons help your gaming experience."
                />
                <div class="tw-flex tw-flex-col tw-gap-5">
                  <label
                    v-for="(addons, index) in addonsOptions"
                    :key="index"
                    class="tw-relative"
                  >
                    <q-checkbox
                      v-model="form.addons"
                      :val="addons"
                      class="step-addons tw-absolute tw-opacity-0 tw-mw-1 tw-mh-1"
                    />
                    <div
                      class="tw-rounded-md tw-cursor-pointer tw-border tw-py-4 tw-px-5 tw-border-light-gray hover:tw-border-purplish-blue focus:tw-border-purplish-blue tw-transition-all tw-flex tw-items-center tw-gap-6"
                    >
                      <div>
                        <q-checkbox
                          v-model="form.addons"
                          :val="addons"
                          class="custom-check"
                        />
                      </div>
                      <div class="tw-flex-1">
                        <h3 class="tw-text-marine-blue tw-font-medium tw-text-base">
                          {{ addons.title }}
                        </h3>
                        <span class="tw-text-cool-gray">{{ addons.description }}</span>
                      </div>
                      <span class="tw-text-purplish-blue">
                        ${{ form.plan_yearly ? `${addons.yearly}/yr` : `${addons.monthly}/mo`}}
                      </span>
                    </div>
                  </label>
                </div>
              </div>
              <StepperNavigation
                :step="step"
                @handleClickNextStep="clickNextStep"
                @handleClickPrevStep="clickPrevStep"
              />
            </div>
          </q-step>

          <q-step
            :name="4"
            title="step 4"
            caption="Summary"
            :done="step > 4"
            color="transparent"
            active-icon="none"
          >
            <div class="tw-min-h-[520px] tw-flex tw-flex-col tw-justify-between">
              <div class="tw-pl-4 tw-py-3">
                <HeaderForm
                  title="Finishing up"
                  subtitle="Double-check everything looks OK before confirming."
                />
                <div class="tw-bg-magnolia tw-rounded-md tw-p-5">
                  <div class="tw-flex tw-space-between tw-items-center tw-justify-between">
                    <div class="tw-flex tw-flex-col tw-items-start">
                      <b class="tw-text-base tw-text-marine-blue">
                        {{ form.plan.label }}
                        ({{ form.plan_yearly ? 'Yearly' : 'Monthly' }})
                      </b>
                      <button
                        @click="step = 2"
                        class="tw-text-cool-gray tw-underline hover:tw-text-marine-blue tw-transition-all">
                        Change
                      </button>
                    </div>
                    <b class="tw-text-base tw-text-marine-blue">
                      {{form.plan_yearly ? `$${form.plan.yearly}/yr` : `$${form.plan.monthly}/mo`}}
                    </b>
                  </div>
                  <template v-if="form.addons.length">
                    <hr class="tw-my-4 tw-border-light-gray" />
                    <div v-for="(addons, index) in form.addons" :key="index">
                      <div class="tw-flex tw-items-center tw-justify-between tw-mt-3">
                      <span class="tw-text-cool-gray">{{ addons.title }}</span>
                      <span class="tw-text-marine-blue">
                        {{form.plan_yearly ? `+$${addons.yearly}/yr` : `+$${addons.monthly}/mo`}}
                      </span>
                    </div>
                  </div>
                  </template>
                </div>
                <div class="tw-flex tw-justify-between tw-items-center tw-p-5 tw-mt-2">
                  <span class="tw-text-cool-gray">
                    Total (per {{ form.plan_yearly ? 'year' : 'month' }})
                  </span>
                  <b class="tw-text-xl tw-text-purplish-blue">
                    +${{ totalPrice }}/{{ form.plan_yearly ? 'yr' : 'mo' }}
                  </b>
                </div>
              </div>
              <StepperNavigation
                :step="step"
                @handleClickNextStep="clickNextStep"
                @handleClickPrevStep="clickPrevStep"
              />
            </div>
          </q-step>

          <q-step
            :name="5"
            title="step 5"
            caption="Thank you!"
            :done="step > 5"
            color="transparent"
            active-icon="none"
          >
            <div class="tw-min-h-[520px] tw-flex tw-flex-col tw-justify-center">
              <div class="tw-text-center">
                <img
                  :src="iconThankYou"
                  alt="Thank you"
                  class="tw-m-auto tw-mb-4"
                />
                <HeaderForm
                  title="Thank you!"
                  subtitle="Thank you for confirming your subscription! We hope you have fun using our platform. If you ever need support, please feel free to email us at support@loremgaming.com."
                />
              </div>
            </div>
          </q-step>
        </q-stepper>
      </q-form>
    </div>
  </q-page>
</template>

<script setup>
  import { computed, ref, watch } from 'vue'
  import HeaderForm from 'src/components/HeaderForm.vue'
  import StepperNavigation from 'src/components/StepperNavigation.vue'
  import { validateEmail } from 'src/utils/formater'

  import iconArcade from 'src/assets/icon-arcade.svg'
  import iconAdvanced from 'src/assets/icon-advanced.svg'
  import iconPro from 'src/assets/icon-pro.svg'
  import iconThankYou from 'src/assets/icon-thank-you.svg'

  const stepper = ref(null)

  const step = ref(1)

  const step1RefName = ref(null)
  const step1RefEmail = ref(null)
  const step1RefPhone = ref(null)

  const plansOptions = [
    {
      label: 'Arcade',
      value: 'arcade',
      monthly: 9,
      yearly: 90,
      label_promo: '2 months free'
    },
    {
      label: 'Advanced',
      value: 'advanced',
      monthly: 12,
      yearly: 120,
      label_promo: '2 months free'
    },
    {
      label: 'Pro',
      value: 'pro',
      monthly: 15,
      yearly: 150,
      label_promo: '2 months free'
    },
  ]

  const addonsOptions = [
    {
      title: 'Online service',
      description: 'Access to multiplayer games',
      monthly: 1,
      yearly: 10,
    },
    {
      title: 'Large storage',
      description: 'Extra 1TB of cloud save',
      monthly: 2,
      yearly: 20,
    },
    {
      title: 'Customizable profile',
      description: 'Custom theme on your profile',
      monthly: 2,
      yearly: 20,
    },
  ]

  const formRef = ref(null)
  const form = ref({
    name: '',
    email: '',
    phone: '',
    plan: plansOptions[0],
    plan_yearly: false,
    addons: [],
  })

  const totalPrice = computed(() => {
    const planPrice = form.value.plan_yearly
      ? form.value?.plan?.yearly
      : form.value?.plan?.monthly

    const totalAddonsPrice = form.value.addons.reduce((acc, addon) => {
      return acc + (form.value.plan_yearly ? addon.yearly : addon.monthly)
    }, 0)

    return planPrice + totalAddonsPrice
  })

  function boxHeight () {
    return { minHeight: 0 }
  }

  function clickPrevStep () {
    stepper.value.previous()
  }

  function getPlanIcon (plan) {
    switch (plan) {
      case 'arcade':
        return 'img:' + iconArcade
      case 'advanced':
        return 'img:' + iconAdvanced
      case 'pro':
        return 'img:' + iconPro
      default:
        return ''
    }
  }

  function clickNextStep () {
    switch (step.value){
      case 1:
        step1RefName.value.validate()
        step1RefEmail.value.validate()
        step1RefPhone.value.validate()
        if(!step1RefName.value.hasError && !step1RefEmail.value.hasError && !step1RefPhone.value.hasError) {
          stepper.value.next()
        }
        break
      case 4:
        stepper.value.next()
        formRef.value.submit()
        break
      default:
        stepper.value.next()
        break
    }
  }

  function submitForm () {
    console.log('Form submitted with values:', form.value)
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
        display: none;
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

  .form-input {
    .q-field {
      .q-field__control {
        border-radius: 10px;

        &::before, &::after {
          border-width: 1px !important;
          border-color: hsl(229, 24%, 87%);
        }
      }

      .q-field__bottom {
        padding: 4px 0 0 0;
      }

      &.q-field--focused, &.q-field--highlighted {
        .q-field__control {
          &::before, &::after {
            border-color: hsl(243, 100%, 62%);
          }
        }
      }

      &.q-field--error {
        .q-field__control {
          &::before, &::after {
            border-color: red;
          }
        }
      }
    }
  }

  .step-plan[aria-checked="true"] + div {
    border-color: hsl(243, 100%, 62%);
    background-color: hsl(217, 100%, 97%);
  }

  .custom-toggle {
    .q-toggle__inner {
      width: 60px;

      &--falsy {
        .q-toggle__thumb {
          left: 16px;
        }
      }

      &--truthy {
        .q-toggle__thumb {
          left: 32px;

          &:after {
            background-color: #fff;
          }
        }
      }
    }

    .q-toggle__track {
      min-height: 19px;
      border-radius: 15px;
      opacity: 1;
      background-color: hsl(213, 96%, 18%);
    }

    .q-toggle__thumb {
      width: 12px;
      height: 12px;
      top: 16px;
    }
  }

  .custom-check {
    .q-checkbox__bg {
      border-radius: 4px;
      border-width: 1px;
      border-color: hsl(229, 24%, 87%);
    }
  }

  .step-addons[aria-checked="true"] + div {
    border-color: hsl(243, 100%, 62%);
    background-color: hsl(217, 100%, 97%);

    .custom-check {
      .q-checkbox__bg {
        background-color: hsl(243, 100%, 62%);
        border-color: hsl(243, 100%, 62%);

        svg {
          transform: scale(0.7);
        }
      }
    }
  }
</style>
