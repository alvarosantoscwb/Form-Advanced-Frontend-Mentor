<template>
  <q-page class="flex flex-center bg-form">
    <form-input
      v-if="step === 1"
      :title="steps[0].title"
      :description="steps[0].description"
      :currentStep="steps[0].currentStep"
      :onNextStep="nextStep"
    />
    <form-input
      v-if="step === 2"
      :title="steps[1].title"
      :description="steps[1].description"
      :currentStep="steps[1].currentStep"
      :onNextStep="nextStep"
      :onPreviousStep="previousStep"
    />
    <form-input
      v-if="step === 3"
      :title="steps[2].title"
      :description="steps[2].description"
      :currentStep="steps[2].currentStep"
      :onNextStep="nextStep"
      :onPreviousStep="previousStep"
    />
    <form-input
      v-if="step === 4"
      :title="steps[3].title"
      :description="steps[3].description"
      :currentStep="steps[3].currentStep"
      :onNextStep="nextStep"
      :onPreviousStep="previousStep"
    />
  </q-page>
</template>

<script setup>
import formInput from "../components/FormInput.vue";
import { ref } from "vue";

const step = ref(1);
const steps = ref([
  {
    title: "Personal info",
    description: "Please provide your name, email address, and phone number.",
    currentStep: 1,
  },
  {
    title: "Select Plan",
    description: "Choose a plan that fits your needs.",
    currentStep: 2,
  },
  {
    title: "Pick add-ons",
    description: "Add ons help enhance your gaming experience.",
    currentStep: 3,
  },
  {
    title: "Finishing up",
    description: "Double-check everything looks OK before confirming.",
    currentStep: 4,
  },
]);

const nextStep = () => {
  if (step.value < 4) {
    step.value += 1;
  }
};
const previousStep = () => {
  if (step.value > 1) {
    step.value -= 1;
  }
};
</script>
