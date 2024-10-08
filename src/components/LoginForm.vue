<script setup>

import {reactive} from "vue";
import {useUserStore} from "@/stores/user";

const store = useUserStore()

let state = reactive({
      login_in_submission: false,
      login_show_alert: false,
      login_alert_variant: "bg-blue-500",
      login_alert_msg: "Please wait! We are logging you in.",
    }
)

const schema = {
  email: "required|min:3|max:100|email",
  password: "required|min:6|max:100",
}

async function login(values) {
  state.login_show_alert = true;
  state.login_in_submission = true;
  state.login_alert_variant = "bg-blue-500";
  state.login_alert_msg = "Please wait! We are logging you in.";

  try {
    await store.authenticate(values)
  } catch (e) {
    state.login_in_submission = false;
    state.login_alert_variant = "bg-red-500";
    state.login_alert_msg = "Invalid login details!";
    return;
  }

  state.login_alert_variant = "bg-green-500";
  state.login_alert_msg = "Success! You are now logged in.";
  window.location.reload()
  console.log(values);
}

</script>

<template>

  <div
      class="text-white text-center font-bold p-4 mb-4"
      v-if="state.login_show_alert"
      :class="state.login_alert_variant"
  >
    {{ state.login_alert_msg }}
  </div>

  <!-- Login Form -->
  <vee-form @submit="login" :validation-schema="schema">
    <!-- Email -->
    <div class="mb-3">
      <label class="inline-block mb-2">Email</label>
      <vee-field
          type="email"
          name="email"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Enter Email"
      />
      <ErrorMessage class="text-red-600" name="email"/>
    </div>
    <!-- Password -->
    <div class="mb-3">
      <label class="inline-block mb-2">Password</label>
      <vee-field
          type="password"
          name="password"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Password"
      />
      <ErrorMessage class="text-red-600" name="password"/>
    </div>
    <button
        type="submit"
        class="block w-full bg-purple-600 text-white py-1.5 px-3 rounded transition hover:bg-purple-700"
        :disabled="state.login_in_submission"
    >
      Submit
    </button>
  </vee-form>
</template>

<style scoped>

</style>