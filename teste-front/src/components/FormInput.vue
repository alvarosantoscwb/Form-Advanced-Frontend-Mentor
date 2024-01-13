<template>
  <div class="antialiased">
    <div class="flex w-full min-h-screen justify-center items-center">
      <div class="bg-white p-4 rounded-xl shadow-2 relative w-[800px]">
        <div class="columns-2 relative mr-36">
          <div class="relative w-fit">
            <img
              class="h-[500px] object-cover rounded-md"
              src="~assets/bg-sidebar-desktop.svg"
            />
            <!-- Adiciona o número acima da imagem com borda redonda -->

            <template v-for="(step, index) in steps" :key="index">
              <div
                :style="{
                  top: step.top + 'px',
                  left: step.left + 'px',
                }"
                class="absolute border-2 text-white border-[#FFFFFF] rounded-full h-8 w-8 flex items-center justify-center font-semibold"
              >
                {{ step.textnum }}
              </div>
              <p
                :style="{
                  top: step.top - 12 + 'px',
                  left: step.left + 30 + 'px',
                }"
                class="ml-4 absolute text-gray-400 rounded-full h-9 flex items-center justify-center text-xs"
              >
                STEP {{ step.number }}
              </p>
              <p
                :style="{
                  top: step.top + 7 + 'px',
                  left: step.left + 30 + 'px',
                }"
                class="ml-4 absolute text-white font-bold rounded-full h-9 flex items-center justify-center text-xs"
              >
                {{ step.text }}
              </p>
            </template>
          </div>

          <div class="flex flex-col space-y-4">
            <div class="space-y-2">
              <div class="space-y-2 w-[400px]">
                <p class="text-3xl font-extrabold mt-8 text-[#02295D]">
                  {{ title }}
                </p>
                <p class="text-sm text-gray-400">{{ description }}</p>
              </div>
            </div>
            <div v-if="currentStep === 1" class="font-semibold text-[#02295D]">
              <p class="mb-1 mt-4">Name</p>
              <q-input outlined dense></q-input>
              <p class="mt-4 mb-1">Email Address</p>
              <q-input outlined dense></q-input>
              <p class="mt-4 mb-1">Phone Number</p>
              <q-input outlined dense></q-input>
            </div>
            <div v-if="currentStep === 2">
              <div class="mr-20">
                <div
                  v-for="(product, index) in products"
                  :key="index"
                  class="border-2 rounded-sm w-full cursor-pointer p-4 flex"
                >
                  <q-img :src="`assets/${product.img}.svg`" />
                  <div>
                    <p class="text-bold">{{ product.name }}</p>
                    <span>{{ product.price }}</span>
                  </div>
                </div>
              </div>
            </div>
            <div v-if="currentStep === 3">
              <div class="border-2 border-[#645FA2] rounded-md">
                <div class="flex p-6">
                  <div class="flex">
                    <q-checkbox
                      v-model="isChecked"
                      color="secondary"
                    ></q-checkbox>
                  </div>
                  <div class="flex">
                    <span class="text-bold text-md text-[#02295D]"
                      >Arcade (Monthly)</span
                    >
                    <p class="text-gray-400">Acess to multiplayer games</p>
                    <div>
                      <div>
                        <p class="text-purple-500 float-right text-bold">
                          +$9/mo
                        </p>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div v-if="currentStep === 4">
              <div class="mt-8">
                <div class="bg-[#F8F9FE] p-4">
                  <div>
                    <span class="text-bold text-md text-[#02295D]"
                      >Arcade (Monthly)</span
                    >
                    <span class="text-[#02295D] float-right mt-1 text-bold"
                      >+$9/mo</span
                    >
                    <p
                      class="text-gray-400 cursor-pointer underline hover:text-[#938CFE]"
                    >
                      Change
                    </p>
                  </div>
                  <div class="mt-8">
                    <span class="text-gray-400">Online service</span>
                    <span class="float-right text-[#02295D]">+$1/mo</span>
                    <div class="mt-4">
                      <span class="text-gray-400">Larger storage</span>
                      <span class="text-[#02295D] float-right">+$2/mo</span>
                    </div>
                  </div>
                  <div class="mt-8">
                    <span class="text-gray-400">Total (per month)</span>
                    <span class="text-purple-500 text-lg text-bold float-right"
                      >+$12/mo</span
                    >
                  </div>
                </div>
              </div>
            </div>
            <div class="pt-14">
              <q-btn
                v-show="currentStep > 1"
                class="text-capitalize bg-gray-100 text-gray-400 float-left outlined rounded-md"
                label="Go Back"
                @click="previousStep"
              ></q-btn>
              <q-btn
                :class="
                  currentStep === 4
                    ? 'bg-[#483EFF] hover:bg-[#938CFE]'
                    : 'bg-[#174A8B]'
                "
                class="text-capitalize text-white float-right w-[100px] rounded-md"
                :label="currentStep === 4 ? 'Confirm' : 'Next Step'"
                @click="nextStep"
              ></q-btn>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps } from "vue";

const props = defineProps({
  title: String,
  description: String,
  currentStep: Number,
  onNextStep: Function,
  onPreviousStep: Function,
});

const email = ref("");
const isChecked = ref("");
const emailExistsError = ref("E-mail já registrado");

const emailRule = ref([
  (v) => !!v || "Email is required",
  (v) => /.+@.+\..+/.test(v) || "Enter a valid email address",
]);

const products = ref([
  { name: "Arcade", img: "icon-arcade", price: "$9" },
  { name: "Advanced", img: "icon-advanced", price: "$12" },
  { name: "Pro", img: "icon-pro", price: "$13" },
]);

const steps = ref([
  { number: 1, top: 30, left: 30, text: "YOUR INFO", textnum: "1" },
  { number: 2, top: 90, left: 30, text: "SELECT PLAN", textnum: "2" },
  { number: 3, top: 150, left: 30, text: "ADD-ONS", textnum: "3" },
  { number: 4, top: 210, left: 30, text: "SUMMARY", textnum: "4" },
]);

const emailAlreadyExists = (newEmail) => {
  const existingEmail = localStorage.getItem("user_email");
  return existingEmail === newEmail;
};
const emailExistsRule = (v) => !emailAlreadyExists(v) || emailExistsError.value;

const previousStep = () => {
  props.onPreviousStep();
};

const nextStep = () => {
  props.onNextStep();
};
</script>
