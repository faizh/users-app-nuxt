<template>
  <div class="p-3">
    <span class="text-center"><h3>Buat Pengguna</h3></span>
    <b-form @submit="onSubmit">
      <b-form-group id="input-group-1" label="Name:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="user.name"
          placeholder="Enter Name"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Email:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="user.email"
          type="email"
          placeholder="Enter Email"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Gender:" label-for="input-3">
        <b-form-select
          id="input-3"
          v-model="user.gender"
          :options="genderList"
          required
        ></b-form-select>
      </b-form-group>

      <b-form-group id="input-group-4" label="Status:" label-for="input-4">
        <b-form-select
          id="input-4"
          v-model="user.status"
          :options="statusList"
          required
        ></b-form-select>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
  </div>
</template>

<script lang="ts">
import { response } from 'express'
import Vue from 'vue'
export default Vue.extend({
  data() {
    return {
      user: {
        name: null,
        email: null,
        gender: null,
        status: null,
      },
      genderList: [
        { text: 'Choose Gender', value: null },
        { text: 'Male', value: 'male' },
        { text: 'Female', value: 'female' },
      ],
      statusList: [
        { text: 'Choose Status', value: null },
        { text: 'Active', value: 'active' },
        { text: 'Inactive', value: 'inactive' },
      ],
    }
  },

  methods: {
    onSubmit(event: any) {
      event.preventDefault()
      let self = this

      const headers = {
        Authorization:
          'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe',
      }

      this.$axios
        .$post(
          'https://gorest.co.in/public/v2/users/',
          {
            name: this.user.name,
            email: this.user.email,
            gender: this.user.gender,
            status: this.user.status,
          },
          { headers: headers }
        )
        .then(function (response) {
          alert('User berhasil dibuat, User ID : ' + response.id)
          self.$router.push({ name: 'users' })
        })
    },
  },
})
</script>
