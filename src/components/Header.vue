<script setup>
import {useStore} from "@/stores/modal";
import {useUserStore} from "@/stores/user";
import {useRouter, useRoute} from 'vue-router'

const router = useRouter()
const route = useRoute()

const modalStore = useStore()
const userStore = useUserStore()

function toggleAuthModal() {
  modalStore.isOpen = !modalStore.isOpen
}

function signOut() {
  userStore.signOut();
  if (route.meta.requiresAuth) {
    router.push({name: 'home'})
  }

}
</script>


<template>
  <!-- Header -->
  <header id="header" class="bg-indigo-600">
    <nav class="container mx-auto flex justify-between items-center py-4 px-4">
      <!-- App Name -->
      <router-link class="text-white font-bold uppercase text-2xl mr-4 px-3 border" href="#"
                   :to="{name: 'home'}" exact-active-class="no-active">Beat
      </router-link>

      <div class="flex items-center">
        <!-- Primary Navigation -->
        <ul class="flex flex-row border py-1 text-lg">
          <!-- Navigation Links -->
          <li v-if="!userStore.userLoggedIn">
            <a class="px-2 text-white" href="#" @click.prevent="toggleAuthModal">Login | Register</a>
          </li>
          <template v-else>
            <li class="border-r">
              <router-link :to="{name: 'manage'}" class="px-2 text-white" href="#">Manage</router-link>
            </li>
            <li>
              <a class="px-2 text-white" href="#" @click.prevent="signOut()">Logout</a>
            </li>
          </template>

        </ul>
      </div>
    </nav>
  </header>
</template>

<style scoped>

</style>