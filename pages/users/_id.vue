<template>
    <div class="p-3">
        <h3 class="text-center">LIHAT PENGGUNA</h3>
        <table>
            <tr>
                <td>NAMA</td>
                <td>:</td>
                <td>{{ user.name }}</td>
            </tr>
            <tr>
                <td>GENDER</td>
                <td>:</td>
                <td>{{ user.gender }}</td>
            </tr>
            <tr>
                <td>EMAIL</td>
                <td>:</td>
                <td>{{ user.email }}</td>
            </tr>
        </table>
        <div class="row">
            <div class="col">
                <span>DAFTAR POST</span>
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
                            <h3>{{post.title}}</h3>
                            {{ post.body }}
                        </b-card-text>
                    </b-col>
                    <b-col md="2">
                        <b-button href="#" variant="danger" class="float-right" @click="confirmDelete(post.id)">Delete</b-button>
                    </b-col>
                </b-row>
            </b-card>
        </div>

        <b-modal v-model="modalShow" title="BUAT POST"  hide-footer>
            <b-form @submit="onSubmit">
                <b-form-group id="input-group-1" label="Title:" label-for="input-1">
                    <b-form-input id="input-1" v-model="formPost.title" placeholder="Enter Title" required></b-form-input>
                </b-form-group>
        
                <b-form-group id="input-group-2" label="Body:" label-for="input-2">
                    <b-form-textarea id="input-2" v-model="formPost.body" placeholder="Enter something..." rows="3"
                        max-rows="6"></b-form-textarea>
                </b-form-group>
                <div>
                    <b-btn @click="modalShow = false">Cancel</b-btn>
                    <b-btn type="submit" variant="primary">Create</b-btn>
                    </div>
            </b-form>
        </b-modal>
    </div>
    
</template>

<script lang="ts">
import e from 'express'
import Vue from 'vue'
export default {
    data() {
        return {
            user: {
                id: null,
                name: null,
                email: null,
                gender: null
            },
            posts: [{
                id: '',
                title: null,
                body: null
            }],
            fields: ['title', 'body', 'actions'],
            modalShow: false,
            formPost: {
                id: '',
                title: null,
                body: null
            }
        }
    },

    async asyncData({params}) {
        const headers= {
            'Authorization' : 'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe'
        }

        const response = await fetch(
            'https://gorest.co.in/public/v2/users/'+params.id
        ).then(res => res.json())

        const post = await fetch(
            'https://gorest.co.in/public/v2/users/'+params.id+'/posts', {headers: headers}
        ).then(res => res.json())

        return {
            user: response,
            posts: post
        }
    },

    methods : {
        confirmDelete(postID: any) {
            let self = this
            const headers= {
                'Authorization' : 'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe'
            }

            this.$axios.$delete(
                'https://gorest.co.in/public/v2/posts/' + postID,
                {headers: headers}
            ).then(function(){
                alert('Post ID : ' + postID + ' has been Deleted!')
                location.reload()
            })
        },

        onSubmit(event: any) {
            event.preventDefault()

            let self = this

            const headers= {
                'Authorization' : 'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe'
            }

            this.$axios.$post(
                'https://gorest.co.in/public/v2/users/'+this.user.id+'/posts', {
                    title: this.formPost.title,
                    body: this.formPost.body
                },
                {headers: headers}
            ).then(function(response){
                alert('Post berhasil dibuat, Post ID : ' + response.id)
                location.reload()
            })
        }
    }
}
</script>