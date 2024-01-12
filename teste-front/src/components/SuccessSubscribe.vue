<template>
  <div class="bg-column flex flex-center" style="width: 100%; height: 100vh">
    <div
      class="bg-white column"
      style="width: 460px; height: 485px; border-radius: 24px"
    >
      <div class="q-ml-xl q-mt-xl">
        <q-img
          src="~assets/icon-list.svg"
          style="height: 55px; width: 55px; border: none"
        />
        <div>
          <h3 class="text-bold q-mt-xl q-mb-md text-sucess">
            Thanks for subscribing!
          </h3>
          <div class="text-sucess">
            <span>A confirmation email has been sent to</span>
            <br />
            <span>
              <span class="text-bold">{{ storageEmail }}</span
              >. Please open it and click</span
            >
            <br />
            <span>the button inside to confirm your subscription</span>
            <br />
            <q-btn
              class="button-form text-white q-mt-xl text-capitalize"
              style="
                width: 360px;
                display: block;
                padding: 15px;
                border-radius: 8px;
              "
              @click="dismiss"
              label="Dismiss message"
            ></q-btn>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";

const router = useRouter();
const storageEmail = localStorage.getItem("user_email");

const dismiss = () => {
  router.push("/");
};
</script>
