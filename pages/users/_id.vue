<template>
  <div class="p-3">
    <span class="text-center"><h3>Lihat Pengguna</h3></span>
    <table>
      <tr>
        <td>Nama</td>
        <td>:</td>
        <td>{{ user.name }}</td>
      </tr>
      <tr>
        <td>Gender</td>
        <td>:</td>
        <td>{{ user.gender }}</td>
      </tr>
      <tr>
        <td>Email</td>
        <td>:</td>
        <td>{{ user.email }}</td>
      </tr>
    </table>
    <div class="row mt-3">
      <div class="col">
        <span class="font-weight-bold" style="font-size: 20px"
          >Daftar Post</span
        >
        <b-btn variant="primary float-right" @click="modalShow = !modalShow">
          BUAT POST
        </b-btn>
      </div>
    </div>
    <div v-for="post in posts" :key="post.id">
      <b-card bg-variant="dark" text-variant="white" class="mt-3">
        <b-row>
          <b-col md="10">
            <b-card-text>
              <h3>{{ post.title }}</h3>
              {{ post.body }}
            </b-card-text>
          </b-col>
          <b-col md="2">
            <b-button
              href="#"
              variant="danger"
              class="float-right"
              @click="confirmDelete(post.id)"
              >Delete</b-button
            >
          </b-col>
        </b-row>
      </b-card>
    </div>

    <b-modal v-model="modalShow" size="lg" title="BUAT POST" hide-footer>
      <b-form @submit="onSubmit">
        <b-form-group id="input-group-1" label="Title:" label-for="input-1">
          <b-form-input
            id="input-1"
            v-model="formPost.title"
            placeholder="Enter Title"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" label="Body:" label-for="input-2">
          <b-form-textarea
            id="input-2"
            v-model="formPost.body"
            placeholder="Enter something..."
            rows="3"
            max-rows="6"
          ></b-form-textarea>
        </b-form-group>
        <div>
          <b-btn @click="modalShow = false">Cancel</b-btn>
          <b-btn type="submit" variant="primary">Create</b-btn>
        </div>
      </b-form>
    </b-modal>

    <b-modal
      title="Notification"
      size="sm"
      buttonSize="sm"
      okVariant="success"
      headerClass="p-2 border-bottom-0"
      footerClass="p-2 border-top-0"
      centered="true"
      v-model="notifModal"
    >
      {{ notifMessage }}
    </b-modal>
  </div>
</template>

<script lang="ts">
import e from 'express'
import Vue from 'vue'
export default Vue.extend({
  data() {
    return {
      user: {
        id: null,
        name: null,
        email: null,
        gender: null,
      },
      posts: [
        {
          id: '',
          title: null,
          body: null,
        },
      ],
      fields: ['title', 'body', 'actions'],
      modalShow: false,
      notifModal: false,
      notifMessage: '',
      formPost: {
        id: '',
        title: null,
        body: null,
      },
    }
  },

  async asyncData({ params }) {
    const headers = {
      Authorization:
        'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe',
    }

    const response = await fetch(
      'https://gorest.co.in/public/v2/users/' + params.id
    ).then((res) => res.json())

    const post = await fetch(
      'https://gorest.co.in/public/v2/users/' + params.id + '/posts',
      { headers: headers }
    ).then((res) => res.json())

    return {
      user: response,
      posts: post,
    }
  },

  methods: {
    confirmDelete(postID: any) {
      let self = this
      const headers = {
        Authorization:
          'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe',
      }

      this.$axios
        .$delete('https://gorest.co.in/public/v2/posts/' + postID, {
          headers: headers,
        })
        .then(function () {
          self.notifMessage = 'Post ID : ' + postID + ' has been Deleted!'
          self.notifModal = !self.notifModal
          self.$nuxt.refresh()
        })
    },

    onSubmit(event: any) {
      event.preventDefault()

      let self = this

      const headers = {
        Authorization:
          'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe',
      }

      this.$axios
        .$post(
          'https://gorest.co.in/public/v2/users/' + this.user.id + '/posts',
          {
            title: this.formPost.title,
            body: this.formPost.body,
          },
          { headers: headers }
        )
        .then(function (response) {
          self.notifMessage = 'Post has been Created! Post ID : ' + response.id
          self.notifModal = !self.notifModal
          self.modalShow = !self.modalShow

          self.formPost.title = null
          self.formPost.body = null
          self.formPost.id = ''

          self.$nuxt.refresh()
        })
    },
  },
})
</script>
