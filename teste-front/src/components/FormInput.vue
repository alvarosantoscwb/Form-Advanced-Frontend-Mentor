<template>
  <div class="antialiased">
    <div class="flex w-full min-h-screen justify-center items-center">
      <div
        class="bg-white flex flex-col md:flex-row md:space-x-6 space-y-6 md:space-y-0 w-full max-w-4xl p-4 rounded-xl shadow-2 relative"
      >
        <!-- Adiciona a borda redonda à imagem -->
        <div class="columns-2 relative">
          <div class="relative">
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
            <p class="text-3xl font-extrabold mt-8 text-[#02295D]">
              {{ title }}
            </p>
            <p class="text-sm text-gray-400">
              {{ description }}
            </p>

            <div v-if="currentStep === 1" class="font-semibold text-[#02295D]">
              <p class="mb-1">Name</p>
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
                  class="border-2 rounded-sm w-[150px] h-[170px] cursor-pointer p-4 flex flex-col items-center justify-between"
                >
                  <img :src="`~${product.img}`" />
                  <div>
                    <p class="text-bold">{{ product.name }}</p>
                    <span>{{ product.price }}</span>
                  </div>
                </div>
              </div>
            </div>
            <div v-if="currentStep === 3">STEP 3</div>
            <div v-if="currentStep === 4">STEP 4</div>
            <div class="text-right mt-[90px]">
              <q-btn
                v-show="currentStep > 1"
                class="text-capitalize bg-[#174A8B] text-white text-medium mr-2"
                label="Back"
                @click="previousStep"
              ></q-btn>
              <q-btn
                class="text-capitalize bg-[#174A8B] text-white text-medium"
                label="Next Step"
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
  { name: "Arcade", img: "assets/icon-arcade.svg", price: "$9" },
  { name: "Advanced", img: "assets/icon-advanced.svg", price: "$12" },
  { name: "Pro", img: "assets/icon-pro.svg", price: "$13" },
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
