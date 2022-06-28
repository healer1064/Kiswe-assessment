<template>
  <div className="header">
    <Logo />
    <SignUpBtn @click="toggleModal" />
  </div>
  <SignUpModal v-if="showModal" :toggleModal="toggleModal" :showToast="showToast" />
  <Toast />
</template>

<script setup>
  import {ref} from 'vue'
  import { defineComponent } from 'vue'
  import { createToast } from 'mosha-vue-toastify';
  import 'mosha-vue-toastify/dist/style.css';

  import SignUpModal from './components/SignUpModal.vue'
  import Logo from './components/Logo.vue'
  import SignUpBtn from './components/SignUpBtn.vue'
  
  const showModal = ref(false)
  
  const toggleModal = () => {
    showModal.value = !showModal.value
  }

  const showToast = (data) => {
    let info = `<br> Email : ${data.value.email.value} <br> Username : ${data.value.username.value} <br> Password : ${data.value.pwd.value} <br> Receiving Email : ${data.value.mail}`
    toggleModal()
    toast(info)
  }

  const toast = (info) => {
    createToast({
      title: 'Signed up sucessfully', 
      description: info
    },{
      type: 'info',
      timeout: 5000,
      showCloseButton: true,
      position: 'top-right',
      transition: 'bounce',
      swipeClose: true,
    })
  }
</script>

<style>

  #app {
    @apply h-screen	w-full py-0 px-8 bg-gray-500	
  }

  .header {
    @apply h-24 w-full flex items-center justify-between
  }

  .logo {
    @apply block my-8 mx-0
  }

  @media (max-width: 350px) {
    .header {
      @apply flex-col
    }
  }
</style>
