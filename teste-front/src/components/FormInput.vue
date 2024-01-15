<template>
  <div
    class="mb-auto md:my-auto bg-transparent w-[58.8rem] min-h-[20rem] rounded-2xl flex flex-col md:flex-row p-4 max-w-md md:max-w-full"
  >
    <div class="flex w-full min-h-screen justify-center items-center">
      <div class="bg-white p-4 rounded-xl shadow-2 relative w-[800px]">
        <div class="columns-2 relative mr-36 space-x-4">
          <div>
            <img class="rounded-md" src="~assets/bg-sidebar-desktop.svg" />
            <!-- Adiciona o número acima da imagem com borda redonda -->

            <div v-for="(step, index) in steps" :key="index">
              <div
                :style="{
                  top: step.top + 'px',
                  left: step.left + 'px',
                }"
                :class="
                  index === currentStep
                    ? 'bg-[#BDE2FD] text-[#02295D]'
                    : 'text-white'
                "
                class="absolute border-2 border-[#FFFFFF] rounded-full h-8 w-8 flex items-center justify-center font-semibold"
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
                STEP {{ step.textnum }}
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
            </div>
          </div>
          <div class="flex flex-col">
            <div>
              <div class="w-[400px]">
                <div class="mt-32" v-if="currentStep === 4">
                  <img class="m-auto" src="~assets/icon-thank-you.svg" />
                </div>
                <p
                  class="text-3xl font-bold mt-8 text-[#02295D]"
                  :class="currentStep === 4 ? 'text-center ' : ''"
                >
                  {{ title }}
                </p>
                <p
                  class="text-sm text-gray-400 mt-1"
                  :class="currentStep === 4 ? 'text-center' : ''"
                >
                  {{ description }}
                </p>
              </div>
            </div>
            <div v-if="currentStep === 0" class="text-[#02295D]">
              <p class="mt-8">Name</p>
              <q-input
                label-color="primary"
                outlined
                dense
                v-model="nameIn"
                :rules="[...nameRule]"
              ></q-input>
              <p>Email Address</p>
              <q-input
                outlined
                dense
                v-model="email"
                :rules="[...emailRule]"
              ></q-input>
              <p>Phone Number</p>
              <q-input
                outlined
                dense
                v-model="phoneNumber"
                :rules="[...phoneRule]"
                mask="(##) #####-####"
              ></q-input>
            </div>
            <div v-if="currentStep === 1">
              <div class="flex w-max">
                <div class="flex items-center gap-3">
                  <div
                    v-for="(product, index) in products"
                    :key="index"
                    @click="selectProduct(index, product)"
                    :class="
                      index === selectedProductIndex ? 'border-[#483EFF]' : ''
                    "
                    class="border-2 mt-8 hover:border-[#483EFF] rounded-xl cursor-pointer p-4 w-[125px]"
                  >
                    <q-img
                      :src="product.img"
                      style="width: 40px; height: 40px"
                    />
                    <div class="pl-1 flex">
                      <p class="text-bold mt-9 mr-5">{{ product.nameMode }}</p>
                      <span class="text-gray-400">{{ product.priceMode }}</span>
                      <p
                        v-if="toggleValue == true"
                        class="text-[#174A8B] text-xs text-bold"
                      >
                        {{ product.free }}
                      </p>
                    </div>
                  </div>
                </div>
              </div>
              <div
                class="mt-9 flex justify-center items-center"
                :class="toggleValue == false ? 'mt-[50px]' : 'mt-[34px]'"
              >
                <p
                  :class="
                    toggleValue == false
                      ? 'text-[#174A8B] text-bold'
                      : 'text-gray-400'
                  "
                >
                  Monthly
                </p>
                <q-toggle
                  class="flex"
                  v-model="toggleValue"
                  color="info"
                  keep-color
                ></q-toggle>
                <p
                  :class="
                    toggleValue == true
                      ? 'text-[#174A8B] text-bold'
                      : 'text-gray-400'
                  "
                >
                  Yearly
                </p>
              </div>
            </div>
            <div v-if="currentStep === 2">
              <div
                class="border-2 w-full rounded-md mb-4 mt-10"
                :class="onlineService == true ? 'border-[#483EFF]' : ''"
              >
                <div class="flex p-4 items-center mt-1">
                  <q-checkbox
                    v-model="onlineService"
                    color="secondary"
                  ></q-checkbox>
                  <div class="flex-col ml-3">
                    <span class="text-bold text-md text-[#02295D]">{{
                      serviceOn.name
                    }}</span>
                    <p class="text-gray-400">{{ serviceOn.description }}</p>
                  </div>
                  <p class="text-purple-500 float-right text-bold ml-[68px]">
                    {{ serviceOn.price }}
                  </p>
                </div>
              </div>
              <div
                class="border-2 w-full rounded-md mb-4 mt-4"
                :class="largerService == true ? 'border-[#483EFF]' : ''"
              >
                <div class="flex p-4 items-center">
                  <q-checkbox
                    v-model="largerService"
                    color="secondary"
                  ></q-checkbox>
                  <div class="flex-col ml-3">
                    <span class="text-bold text-md text-[#02295D]">{{
                      largerOn.name
                    }}</span>
                    <p class="text-gray-400">{{ largerOn.description }}</p>
                  </div>
                  <p class="text-purple-500 float-right text-bold ml-[98px]">
                    {{ largerOn.price }}
                  </p>
                </div>
              </div>
              <div
                class="border-2 w-full rounded-md mb-4 mt-4"
                :class="customService == true ? 'border-[#483EFF]' : ''"
              >
                <div class="flex p-4 items-center">
                  <q-checkbox
                    v-model="customService"
                    color="secondary"
                  ></q-checkbox>
                  <div class="flex-col ml-3">
                    <span class="text-bold text-md text-[#02295D]">{{
                      customOn.name
                    }}</span>
                    <p class="text-gray-400">{{ customOn.description }}</p>
                  </div>
                  <p class="text-purple-500 float-right text-bold ml-[65px]">
                    {{ customOn.price }}
                  </p>
                </div>
              </div>
            </div>
            <div v-if="currentStep === 3">
              <div>
                <div
                  class="bg-[#F8F9FE]"
                  :class="
                    selectedModesData.length == 2 ? 'mt-[40px] mb-[65px]' : ''
                  "
                  :style="
                    selectedModesData.length == 1
                      ? 'margin-top: 40px; margin-bottom: 102px;'
                      : ''
                  "
                >
                  <div
                    v-for="(item, index) in saveValues"
                    :key="index"
                    :class="
                      selectedModesData.length == 3 ? 'mt-[40px] mb-[40px]' : ''
                    "
                  >
                    <span class="text-bold text-md text-[#02295D]">{{
                      item.nameMode
                    }}</span>
                    <span class="text-[#02295D] float-right mt-1 text-bold">{{
                      item.priceMode
                    }}</span>
                    <p
                      class="text-gray-400 cursor-pointer underline hover:text-[#938CFE]"
                      @click="changeMode"
                    >
                      Change
                    </p>
                  </div>
                  <div
                    class="mt-4"
                    v-for="(item, index) in selectedModesData"
                    :key="index"
                  >
                    <span class="text-gray-400">{{ item.name }}</span>
                    <span class="float-right text-[#02295D]">{{
                      item.price
                    }}</span>
                  </div>
                  <div class="mt-8">
                    <span class="text-gray-400">{{ totalTitle }}</span>
                    <span
                      class="text-purple-500 text-lg text-bold float-right"
                      >{{ totalMonthlyPrice }}</span
                    >
                  </div>
                </div>
              </div>
            </div>
            <div
              :class="currentStep == 2 ? 'mt-8' : 'mt-36 '"
              v-if="currentStep != 4"
            >
              <q-btn
                v-show="currentStep > 0"
                class="text-capitalize bg-gray-100 pb-2 pt-2 text-gray-400 float-left w-[100px] outlined rounded-md"
                label="Go Back"
                @click="previousStep"
              ></q-btn>
              <q-btn
                :class="
                  currentStep === 3
                    ? 'bg-[#483EFF] hover:bg-[#938CFE]'
                    : 'bg-[#174A8B]'
                "
                class="text-capitalize text-white float-right w-[100px] p-2 rounded-md"
                :label="currentStep === 3 ? 'Confirm' : 'Next Step'"
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
import {
  ref,
  defineProps,
  computed,
  watchEffect,
  onMounted,
  onUnmounted,
} from "vue";
import iconAdvanced from "../assets/icon-advanced.svg";
import iconArcade from "../assets/icon-arcade.svg";
import iconPro from "../assets/icon-pro.svg";

const props = defineProps({
  title: String,
  description: String,
  currentStep: Number,
  onNextStep: Function,
  onPreviousStep: Function,
  onChangeMode: Function,
});

onMounted(() => {
  const savedIndex = localStorage.getItem("selectedProductIndex");
  if (savedIndex !== null) {
    selectedProductIndex.value = parseInt(savedIndex);
  }
  if (props.currentStep == 4) {
    localStorage.clear();
    customService.value = false;
    largerService.value = false;
    onlineService.value = false;
  }
  updateLocalStorage();
});

const customService = ref(
  JSON.parse(localStorage.getItem("customService")) || false
);
const email = ref(JSON.parse(localStorage.getItem("email")) || "");

const emailRule = ref([
  (v) => !!v || "Email is required",
  (v) => /.+@.+\..+/.test(v) || "this field is required",
]);
const onlineService = ref(
  JSON.parse(localStorage.getItem("onlineService")) || false
);
const largerService = ref(
  JSON.parse(localStorage.getItem("largerService")) || false
);
const nameIn = ref(JSON.parse(localStorage.getItem("nameIn")) || "");
const phoneNumber = ref(JSON.parse(localStorage.getItem("phoneNumber")) || "");
const phoneRule = ref([
  (v) => !!v || "Phone number is required",
  (v) =>
    /^\d+$/.test(v.replace(/[()\-\s]/g, "")) ||
    "Phone number must contain only numbers",
]);
const nameRule = ref([
  (v) => !!v || "Name is required",
  (v) => v.length >= 4 || "Name must be at least 4 characters",
]);
const saveValues = ref(JSON.parse(localStorage.getItem("saveValues")) || []);
const selectedProductIndex = ref(-1);
const selectedModesData = ref(
  JSON.parse(localStorage.getItem("selectedModes")) || []
);
const steps = ref([
  { number: 0, top: 30, left: 30, text: "YOUR INFO", textnum: "1" },
  { number: 1, top: 90, left: 30, text: "SELECT PLAN", textnum: "2" },
  { number: 2, top: 150, left: 30, text: "ADD-ONS", textnum: "3" },
  { number: 3, top: 210, left: 30, text: "SUMMARY", textnum: "4" },
]);
const toggleValue = ref(
  JSON.parse(localStorage.getItem("toggleValue")) || false
);

const products = computed(() => {
  if (toggleValue.value == false) {
    return [
      {
        nameMode: "Arcade",
        img: iconArcade,
        priceMode: "$9/mo",
        totalTitle: "Total (per month)",
      },
      {
        nameMode: "Advanced",
        img: iconAdvanced,
        priceMode: "$12/mo",
        totalTitle: "Total (per month)",
      },
      {
        nameMode: "Pro",
        img: iconPro,
        priceMode: "$13/mo",
        totalTitle: "Total (per month)",
      },
    ];
  } else {
    return [
      {
        nameMode: "Arcade",
        img: iconArcade,
        priceMode: "$90/yr",
        free: "2 months free",
        totalTitle: "Total (per year)",
      },
      {
        nameMode: "Advanced",
        img: iconAdvanced,
        priceMode: "$120/yr",
        free: "2 months free",
        totalTitle: "Total (per year)",
      },
      {
        nameMode: "Pro",
        img: iconPro,
        priceMode: "$150/yr",
        free: "2 months free",
        totalTitle: "Total (per year)",
      },
    ];
  }
});
const largerOn = computed(() => {
  let sub = [
    {
      name: "Larger storage",
      description: "Extra 1TB of cloud save",
      price: toggleValue.value == false ? "+$2/mo" : "+$20/yr",
    },
  ];
  return sub[0];
});
const customOn = computed(() => {
  let sub = [
    {
      name: "Customizable profile",
      description: "Custom theme on your profile",
      price: toggleValue.value == false ? "+$2/mo" : "+$20/yr",
    },
  ];
  return sub[0];
});
const serviceOn = computed(() => {
  let sub = [
    {
      name: "Online service",
      description: "Access to multiplayer games",
      price: toggleValue.value == false ? "+$1/mo" : "+$10/yr",
    },
  ];
  return sub[0];
});
const totalMonthlyPrice = computed(() => {
  let basePrice = 0;
  let saveType = "";

  let addonTotal = 0;
  selectedModesData.value.forEach((item) => {
    addonTotal += parseFloat(item.price.replace(/\D/g, ""));
  });
  let modeTotal = 0;
  saveValues.value.forEach((mode) => {
    if (mode.priceMode.includes("/mo")) {
      saveType = mode.priceMode.slice(3);
    } else {
      saveType = mode.priceMode.slice(4);
    }
    modeTotal += parseFloat(mode.priceMode.replace(/\D/g, ""));
  });
  if (saveType.includes("/mo")) {
    return `+$${basePrice + addonTotal + modeTotal + saveType}`;
  } else {
    return `$${basePrice + addonTotal + modeTotal + saveType}`;
  }
});
const totalTitle = computed(() => {
  let total = "";
  saveValues.value.forEach((item) => {
    total = item.totalTitle;
  });
  return total;
});

const changeMode = () => {
  customService.value = false;
  largerService.value = false;
  onlineService.value = false;
  localStorage.removeItem("saveValues");
  localStorage.removeItem("selectedProductIndex");
  updateLocalStorage();
  props.onChangeMode();
};
const updateLocalStorage = () => {
  const selectedModesDataValue = [];

  if (onlineService.value) {
    selectedModesDataValue.push(serviceOn.value);
  }
  if (largerService.value) {
    selectedModesDataValue.push(largerOn.value);
  }
  if (customService.value) {
    selectedModesDataValue.push(customOn.value);
  }

  selectedModesData.value = selectedModesDataValue;
  localStorage.setItem(
    "selectedModes",
    JSON.stringify(selectedModesData.value)
  );
  localStorage.setItem("onlineService", JSON.stringify(onlineService.value));
  localStorage.setItem("largerService", JSON.stringify(largerService.value));
  localStorage.setItem("customService", JSON.stringify(customService.value));
};
const nextStep = () => {
  if (props.currentStep === 0) {
    const isEmailValid = emailRule.value.every(
      (rule) => rule(email.value) === true
    );
    const isNameValid = nameRule.value.every(
      (rule) => rule(nameIn.value) === true
    );
    const isPhoneValid = phoneRule.value.every(
      (rule) => rule(phoneNumber.value) === true
    );

    if (!isEmailValid || !isNameValid || !isPhoneValid) {
      return;
    }

    localStorage.setItem("nameIn", JSON.stringify(nameIn.value));
    localStorage.setItem("email", JSON.stringify(email.value));
    localStorage.setItem("phoneNumber", JSON.stringify(phoneNumber.value));
    props.onNextStep();
  }
  if (props.currentStep === 1) {
    if (selectedProductIndex.value == -1) {
      return;
    } else props.onNextStep();
  }
  if (props.currentStep === 2) {
    if (
      largerService.value == false &&
      onlineService.value == false &&
      customService.value == false
    ) {
      return;
    } else props.onNextStep();
  }
  if (props.currentStep === 3) {
    props.onNextStep();
  }
  updateLocalStorage();
};
const previousStep = () => {
  props.onPreviousStep();
  if (props.currentStep == 2) {
    localStorage.removeItem("saveValues");
    localStorage.removeItem("selectedProductIndex");
  }
  updateLocalStorage();
};
const selectProduct = (index, product) => {
  selectedProductIndex.value = index;
  saveValues.value = [product];
  localStorage.setItem("saveValues", JSON.stringify(saveValues.value));
  localStorage.setItem("selectedProductIndex", index);
};

watchEffect(() => {
  localStorage.setItem("toggleValue", JSON.stringify(toggleValue.value));
});
</script>
