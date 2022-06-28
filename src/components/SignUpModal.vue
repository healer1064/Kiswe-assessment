<template>
  <div class="py-10">
    <div class="modal-container" @click="toggleModal">
      <div class="w-auto my-6 mx-8 max-w-6xl absolute top-10" @click="stopPropagation">
        <!--content-->
        <div class="modal-content">
          <!--header-->
          <div class="modal-header">
            <button class="close-btn" @click="toggleModal">
              <span class="text-white h-6 w-6 text-2xl block outline-none focus:outline-none">
                ×
              </span>
            </button>
          </div>
          <!--body-->
          <div class="relative flex-auto rounded-b">
            <div class="mx-auto p-4 bg-neutral-800 text-gray-300">
              <div class="w-full md:w-3/4 lg:w-3/4 mx-auto">
                <h1 class="text-lg text-center">Create a new account</h1>
                <form class="flex flex-col mt-4" @submit.prevent="submit">
                  <div class="form-input-container">
                    <label class="font-bold" for="email">Email</label>
                    <div class="form-input">
                      <input
                        name="email"
                        v-model="form.email.value"
                        className="pr-8 "
                        @keyup="validate"
                      />
                      <span className="w-4 h-4 absolute top-2.5 right-2.5">
                        <IdentificationIcon className="pointer-events-none" />
                      </span>
                    </div>
                    <div v-if="form.email.err" className="my-2 text-sm text-neutral-400">{{form.email.err}}</div>
                  </div>
                  <div class="form-input-container">
                    <label class="font-bold mt-4" for="confirm_email">Confirm Email</label>
                    <div class="form-input">
                      <input
                        name="confirm_email"
                        v-model="form.confirm_email.value"
                        @keyup="validate"
                      />
                    </div>
                    <div v-if="form.confirm_email.err" className="my-2 text-sm text-neutral-400">{{form.confirm_email.err}}</div>
                  </div>
                  <div class="form-input-container">
                    <label class="font-bold mt-4" for="username">Username</label>
                    <div class="form-input">
                      <input
                        type="text"
                        name="username"
                        v-model="form.username.value"
                        @keyup="validate"
                      />
                    </div>
                    <div v-if="form.username.err" className="my-2 text-sm text-neutral-400 md:w-[498px]">{{form.username.err}}</div>
                  </div>
                  <div class="form-input-container">  
                    <label class="font-bold mt-4" for="password">Password</label>
                    <div class="form-input">
                      <input
                        type="password"
                        name="pwd"
                        v-model="form.pwd.value"
                        @keyup="validate"
                      />
                    </div>
                    <div v-if="form.pwd.err" className="my-2 text-sm text-neutral-400 md:w-[498px]">{{form.pwd.err}}</div>
                  </div>
                  <div class="form-input-container">  
                    <label class="font-bold mt-4" for="password">Confirm Password</label>
                    <div class="form-input">
                      <input
                        type="password"
                        name="confirm_pwd"
                        v-model="form.confirm_pwd.value"
                        @keyup="validate"
                      />
                      <span class="w-4 h-4 absolute top-2.5 right-2.5">
                        <LockClosedIcon className="pointer-events-none" />
                      </span>
                    </div>
                    <div v-if="form.confirm_pwd.err" className="my-2 text-sm text-neutral-400">{{form.confirm_pwd.err}}</div>
                  </div>
                  <div class="flex form-input">
                    <label class="font-bold">
                      <input 
                        type="checkbox"
                        class="checked:bg-blue-500 ..."
                        name="mail"
                        v-model="form.mail" 
                      />
                      <span class="text-sm">I want to receive emails from VenewLive and its partners about upcoming shows and information.</span>
                    </label>
                  </div>
                  <div class="flex form-input">
                    <label class="font-bold">
                      <input 
                        type="checkbox"
                        class="checked:bg-blue-500 ..."
                        v-model="form.agreement" 
                        name="agreement"
                      />
                      <span class="text-sm">By submitting this form I agree to the <span className="underline decoration-1 cursor-pointer">Terms of Service</span> and <span className="underline decoration-1 cursor-pointer">Privacy Notice</span>.</span>
                    </label>
                  </div>
                  <div className="w-full flex items-center justify-center">
                    <button
                      class="submit-btn"
                    >
                      Submit
                    </button>
                  </div>
                </form>
              </div>
            </div>
          </div>
          <!--footer-->
          <div class="p-6 bg-neutral-800 rounded-b-xl" /> 
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import {ref} from 'vue'
  import { IdentificationIcon, LockClosedIcon } from '@heroicons/vue/solid'

  const props = defineProps({
    toggleModal: {
      type: Function,
      default: () => null
    },
    showToast: {
      type: Function,
      default: () => null
    }
  })

  const form = ref({
    email: {
      value: '',
      err: ''
    },
    confirm_email: {
      value: '',
      err: ''
    },
    username: {
      value: '',
      err: ''
    },
    pwd: {
      value: '',
      err: ''
    },
    confirm_pwd: {
      value: '',
      err: ''
    },
    mail: true,
    agreement: false
  })

  const stopPropagation = (e) => {
    e.stopPropagation()
  }
      
  const isValidFormElement = (formElement) => {
    return formElement.value !== "" && formElement.err === ""
  }
  const submit = () => {
    for (const [key, value] of Object.entries(form.value)) {
      if(key !== 'mail' && key !== 'agreement' && !isValidFormElement(value))
        return
    }
    if(!form.value.agreement)
      return
    props.showToast(form)
  }
  const validate = (e) => {
    e.preventDefault()
    let field = e.target.name
    let value = e.target.value
    form.value[field].err = ""
    const pattern_email = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    const pattern_pwd = /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[a-zA-Z]).{8,}$/;
    const pattern_username = /^(?=.{3,20}$)([a-zA-Z0-9]*_?[a-zA-Z0-9]*)$/;

    if (value && value.length < 2)
      return;
    else {
      switch(field) {
        case "email":
          if (!pattern_email.test(value.toLowerCase()))
            form.value[field].err = "Invalid email type"
          break;
        case "confirm_email":
          if(value !== form.value.email.value)
            form.value.confirm_email.err = "It must be same with email"
          break;
        case "username":
          if(!pattern_username.test(value))
            form.value[field].err = "Username requires 3 to 20 characters using letters, digits and optionally a single punctuation _ characters."
          break;
        case "pwd":
          if(!pattern_pwd.test(value))
            form.value[field].err = "Password must be at least 8 characters long, contain one lowercase letter, contain one uppercase letter and include at least one number (0-9)."
          break;
        case "confirm_pwd":
          if(value !== form.value.pwd.value)
            form.value.confirm_pwd.err = "It must be same with password"
          break;
      }
    }
  }
</script>

<style scoped>
input {
  @apply px-4 py-2 w-full rounded-md bg-neutral-600 border-transparent text-sm
}
.modal-container {
  @apply overflow-x-hidden overflow-y-auto fixed inset-0 z-50 outline-none focus:outline-none justify-center items-center flex
}
.modal-content {
  @apply border-0 rounded-xl shadow-lg relative flex flex-col w-full bg-white outline-none focus:outline-none
}
.modal-header {
  @apply flex items-start justify-between p-1 rounded-t-xl bg-neutral-800
}
.close-btn {
  @apply p-1 ml-auto border-0 text-white float-right text-3xl
}
.submit-btn {
  @apply mt-4 mb-8 px-6 py-2 w-max  leading-6 text-base rounded-md border border-transparent bg-fuchsia-900 text-blue-100 hover:text-white
}
.form-input-container {
  @apply mb-4
}
.form-input {
  @apply relative min-h-[36px] mt-1
}
.form-input > input {
  @apply relative w-full top-0 left-0
}
.form-input input[type="checkbox"] {
  @apply w-auto mr-2
}
</style>