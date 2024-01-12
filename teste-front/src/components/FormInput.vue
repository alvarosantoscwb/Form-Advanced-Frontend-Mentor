<template>
  <div
    class="bg-white column inline"
    style="width: 800px; height: 550px; border-radius: 16px"
  >
    <div class="q-mt-lg">
      <h3 class="text-bold q-mb-md text-center">Stay updated!</h3>
      <div class="q-ml-md" style="font-weight: 500">
        <div style="padding-left: 35px">
          <span
            >Join 60.000+ product managers receiving monthly<br />
            updates on:</span
          >
        </div>
        <div class="q-mt-md" style="margin-left: 34px">
          <div
            class="q-mt-md"
            v-for="list in labelArray"
            :key="list.id"
            style="display: flex; align-items: center"
          >
            <q-img
              class="q-mr-md"
              src="~assets/icon-list.svg"
              style="height: 18px; width: 18px"
            />
            <span>{{ list }}</span>
          </div>
        </div>
        <div style="margin-left: 34px; padding-top: 32px">
          <label
            class="q-mb-sm text-bold"
            style="display: block; text-align: left; font-size: 11px"
            >Email address</label
          >
          <q-input
            v-model="email"
            outlined
            dense
            placeholder="email@company.com"
            type="email"
            color="black"
            :style="$q.screen.gt.xs ? 'width: 330px' : 'width: 310px'"
            :rules="[...emailRule, emailExistsRule]"
          ></q-input>
          <q-btn
            class="button-form text-white q-mt-xs text-capitalize"
            :style="$q.screen.gt.xs ? 'width: 330px' : 'width: 310px'"
            style="display: block; padding: 10px; border-radius: 8px"
            @click="subscribe"
            label="subscribe to monthly newsletter"
          ></q-btn>
        </div>
      </div>
    </div>

    <div v-if="$q.screen.gt.xs" class="flex q-mt-md">
      <q-img
        src="~assets/illustration-sign-up-desktop.svg"
        style="height: 100%; height: 500px; width: 350px; margin: 12px"
        fit
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import { useQuasar } from "quasar";

const $q = useQuasar();
const router = useRouter();

const email = ref("");
const emailExistsError = ref("E-mail já registrado");

const emailRule = ref([
  (v) => !!v || "Email is required",
  (v) => /.+@.+\..+/.test(v) || "Enter a valid email address",
]);

const labelArray = ref([
  "Product discovery and building what matters",
  "Measuring to ensure updates are a success",
  "And much more!",
]);

const emailAlreadyExists = (newEmail) => {
  const existingEmail = localStorage.getItem("user_email");
  return existingEmail === newEmail;
};
const emailExistsRule = (v) => !emailAlreadyExists(v) || emailExistsError.value;

const subscribe = () => {
  if (email.value && email.value.includes("@") && email.value.includes(".")) {
    if (emailAlreadyExists(email.value)) {
      emailExistsError.value = "E-mail já registrado";
    } else {
      emailExistsError.value = ""; // Limpa a mensagem de erro se não houver erro
      localStorage.setItem("user_email", email.value);
      router.push("/sucess-subscribe");
    }
  }
};
</script>
