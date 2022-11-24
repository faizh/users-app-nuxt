<template>
    <div>
        <div class="content p-3">
            <span class="text-center"><h3>DAFTAR PENGGUNA</h3></span>
            <nuxt-link :to="{name: 'users-create'}" class="btn btn-add btn-primary float-right mb-2">Buat Pengguna</nuxt-link>

            <b-table striped hover :items="users" :fields="fields">
                <template #cell(actions)="data">
                    <span>
                        <nuxt-link :to="'users/' + data.item.id">
                            <b-btn class="btn btn-info">View</b-btn>
                        </nuxt-link>
                        <nuxt-link :to="'users/update/' + data.item.id">
                            <b-btn class="btn btn-warning">Update</b-btn>
                        </nuxt-link>
                        <b-btn class="btn btn-danger" @click="confirmDelete(data.item.id)">Delete</b-btn>
                    </span>                
                </template>
            </b-table>

            <b-pagination v-model="currentPage" :total-rows="rows" :per-page="perPage" align="center">
            </b-pagination>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default {
    data (){
        return {
            users: null,
            fields: ['id', 'name', 'email', 'gender', 'status', 'actions'],
            currentPage: 1,
            rows: 100,
            perPage: 10,
            param: ''
        }
    },

    async asyncData(){
        const response = await fetch(
            'https://gorest.co.in/public/v2/users'
        ).then(res => res.json())

        return {users: response}

        // this.users = await this.$axios.$get(
        //     'https://gorest.co.in/public/v2/users'
        // )
    },

    methods : {
        confirmDelete(userID: any) {
            let self = this
            const headers= {
                'Authorization' : 'Bearer de6a595b6394115dd48ebbc69b32ccd30d76968c34ee995db68288393d5ecfbe'
            }

            this.$axios.$delete(
                'https://gorest.co.in/public/v2/users/' + userID,
                {headers: headers}
            ).then(function(){
                alert('User ID : ' + userID + ' has been Deleted!')
                self.$nuxt.refresh()
            })
        },

        async getDataPagination() {
            this.users = await this.$axios.$get(
                'https://gorest.co.in/public/v2/users' + this.param
            )
        },
        
    },

    watch : {
        currentPage: {
            handler: function(value){
                this.param= '?page='+value+'&per_page='+this.perPage
                this.getDataPagination()
            }
        }
    }
}
</script>