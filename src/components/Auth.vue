<script setup>
import {useStore} from "@/stores/modal";
import {computed, reactive} from 'vue'
import RegisterForm from "@/components/RegisterForm.vue";
import LoginForm from "@/components/LoginForm.vue";

const store = useStore()

let state = reactive({
      tab: 'login',
    }
)

const isHidden = computed(() => {
  return (store.isOpen) ? '' : 'hidden'
})

const modalCloseHandler = () => {
  store.isOpen = false
}

const changeTabHandler = (name) => {
  state.tab = name
}

</script>

<template>
  <!-- Auth Modal -->
  <div :class="isHidden" class="fixed z-10 inset-0 overflow-y-auto" id="modal">
    <div
        class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
    >
      <div @click="modalCloseHandler" class="fixed inset-0 transition-opacity">
        <div class="absolute inset-0 bg-gray-800 opacity-75"></div>
      </div>

      <!-- This element is to trick the browser into centering the modal contents. -->
      <span class="hidden sm:inline-block sm:align-middle sm:h-screen"
      >&#8203;</span
      >

      <div
          class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full"
      >
        <!-- Add margin if you want to see some of the overlay behind the modal-->
        <div class="py-4 text-left px-6">
          <!--Title-->

          <div class="flex items-center justify-center pb-5">
            <!-- Modal Close Button -->
            <p class="text-2xl font-bold">Your Account</p>
            <span @click.prevent="modalCloseHandler" class="modal-close cursor-pointer z-50 ml-auto absolute right-5 top-2.5">
              <i class="fas fa-times"></i>
            </span>
          </div>

          <!-- Tabs -->
          <ul class="flex flex-wrap mb-4">
            <li class="flex-auto text-center">
              <a
                  :class="state.tab === 'login' ? 'hover:text-white text-white bg-blue-600': 'hover:text-blue-600'"
                  @click.prevent="changeTabHandler('login')"
                  class="block rounded py-3 px-4 transition "
                  href="#"
              >Login</a
              >
            </li>
            <li class="flex-auto text-center">
              <a @click.prevent="changeTabHandler('registration')"
                 :class="state.tab === 'registration' ? 'hover:text-white text-white bg-blue-600': 'hover:text-blue-600'"
                 class="block rounded py-3 px-4 transition" href="#"
              >Register</a
              >
            </li>
          </ul>

          <login-form v-if="state.tab === 'login'"/>
          <register-form v-else/>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>