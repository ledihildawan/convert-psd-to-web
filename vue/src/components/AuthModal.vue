<template>
  <Modal>
    <template v-slot:heading>
      <p class="text-2xl font-bold">Your Account</p>
    </template>

    <!-- Tabs -->
    <ul class="flex flex-wrap mb-4">
      <li class="flex-auto text-center">
        <a
          @click.prevent="tab = 'login'"
          class="block rounded py-3 px-4 transition"
          href="#"
          :class="tab === 'login' ? 'bg-blue-600 hover:text-white text-white' : ''"
          >Login</a
        >
      </li>
      <li class="flex-auto text-center">
        <a
          @click.prevent="tab = 'register'"
          class="block rounded py-3 px-4 transition"
          href="#"
          :class="tab === 'register' ? 'bg-blue-600 hover:text-white text-white' : ''"
          >Register</a
        >
      </li>
    </ul>

    <!-- Login Form -->
    <form v-show="tab === 'login'">
      <!-- Email -->
      <div class="mb-3">
        <label class="inline-block mb-2">Email</label>
        <input
          type="email"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Enter Email"
        />
      </div>
      <!-- Password -->
      <div class="mb-3">
        <label class="inline-block mb-2">Password</label>
        <input
          type="password"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Password"
        />
      </div>
      <button
        type="submit"
        class="block w-full bg-purple-600 text-white py-1.5 px-3 rounded transition hover:bg-purple-700"
      >
        Submit
      </button>
    </form>

    <!-- Registration Form -->
    <vee-form v-show="tab === 'register'" :validation-schema="schema" @submit="register">
      <!-- Name -->
      <div class="mb-3">
        <label class="inline-block mb-2">Name</label>
        <vee-field
          name="name"
          type="text"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Enter Name"
        />
        <ErrorMessage class="text-red-600" name="name"></ErrorMessage>
      </div>
      <!-- Email -->
      <div class="mb-3">
        <label class="inline-block mb-2">Email</label>
        <vee-field
          name="email"
          type="email"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Enter Email"
        />
        <ErrorMessage class="text-red-600" name="email"></ErrorMessage>
      </div>
      <!-- Age -->
      <div class="mb-3">
        <label class="inline-block mb-2">Age</label>
        <vee-field
          name="age"
          type="number"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Age"
        />
        <ErrorMessage class="text-red-600" name="age"></ErrorMessage>
      </div>
      <!-- Password -->
      <div class="mb-3">
        <label class="inline-block mb-2">Password</label>
        <vee-field name="password" :bails="false" v-slot="{ field, errors }">
          <input
            type="password"
            placeholder="Password"
            class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
            v-bind="field"
          />
          <div class="text-red-600" v-for="(error, $eIdx) in errors" :key="`error-{$eIdx}`">
            {{ error }}
          </div>
        </vee-field>
      </div>
      <!-- Confirm Password -->
      <div class="mb-3">
        <label class="inline-block mb-2">Confirm Password</label>
        <vee-field
          name="confirm_password"
          type="password"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
          placeholder="Confirm Password"
        />
        <ErrorMessage class="text-red-600" name="confirm_password"></ErrorMessage>
      </div>
      <!-- Country -->
      <div class="mb-3">
        <label class="inline-block mb-2">Country</label>
        <vee-field
          as="select"
          name="country"
          class="block w-full py-1.5 px-3 text-gray-800 border border-gray-300 transition duration-500 focus:outline-none focus:border-black rounded"
        >
          <option value="USA">USA</option>
          <option value="Mexico">Mexico</option>
          <option value="Germany">Germany</option>
        </vee-field>
        <ErrorMessage class="text-red-600" name="country"></ErrorMessage>
      </div>
      <!-- TOS -->
      <div class="mb-3 pl-6">
        <vee-field
          name="tos"
          value="1"
          type="checkbox"
          class="w-4 h-4 float-left -ml-6 mt-1 rounded"
        />
        <label class="inline-block">Accept terms of service</label>
        <ErrorMessage class="text-red-600 block" name="tos"></ErrorMessage>
      </div>
      <button
        type="submit"
        class="block w-full bg-purple-600 text-white py-1.5 px-3 rounded transition hover:bg-purple-700"
      >
        Submit
      </button>
    </vee-form>
  </Modal>
</template>

<script lang="ts">
import { mapState, mapWritableState } from 'pinia'

import useModalStore from '@/stores/modal'
import Modal from '@/components/shared/Modal.vue'

export default {
  name: 'AuthModal',
  components: {
    Modal
  },
  data() {
    return {
      tab: 'login',
      schema: {
        name: 'required|min:3|max:100|alpha_spaces',
        email: 'required|min:3|max:100|email',
        age: 'required|min_value:18|max_value:100',
        password: 'required|min:9|max:100',
        confirm_password: 'confirmed:@password',
        country: 'required',
        tos: 'required'
      }
    }
  },
  computed: {
    ...mapWritableState(useModalStore, {
      modalVisibility: 'isOpen'
    }),
    ...mapState(useModalStore, ['hiddenClass'])
  },
  methods: {
    register(value) {
      console.log(value)
    }
  }
}
</script>
