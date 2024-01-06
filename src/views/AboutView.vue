<template>
  <div class="about">
    <div class="grid grid-cols-1 gap-2">
      <div class="flex items-center justify-center">
        <button
          @click="handleGoogleLogin"
          type="button"
          class="bg-blue-600 text-white hover:bg-blue-700 py-2 px-4 rounded-full flex items-center me-3"
        >
          Login with Google
        </button>
        <button
          type="button"
          class="bg-blue-600 text-white hover:bg-blue-700 py-2 px-4 rounded-full flex items-center"
        >
          Login with Facebook
        </button>
      </div>
      <div class="flex">{{ user }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Loading from "@/components/Loading.vue"
import {
  getAuth,
  signInWithPopup,
  signOut,
  signInWithRedirect,
  GoogleAuthProvider,
  signInWithCredential,
  getRedirectResult,
} from "firebase/auth"
import { onMounted, ref, type Ref } from "vue"
import firebaseApp from "@/firebaseConfig"

const provider = new GoogleAuthProvider()
const auth = getAuth()
const user: Ref<string | null> = ref(null)
const userCred: Ref<any> = ref(null)

onMounted(async () => {
  firebaseApp
  userCred.value = await getRedirectResult(auth) as any
  console.log(userCred.value.user)
  user.value = userCred.value.user.displayName
})

const handleGoogleLogin = async () => {
  console.log("google login")
  signInWithRedirect(auth, provider)

  // signInWithRedirect(auth, provider)
  //   .then((result) => {
  //     // const user = result.user;
  //     // console.log(result.user.displayName)
  //     console.log(result)
  //     // user.value = result
  //   })
  //   .catch((error) => {
  //     console.log(error)
  //   })
}
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
