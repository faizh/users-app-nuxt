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
    </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default {
    data() {
        return {
            user: {
                name: null,
                email: null,
                gender: null
            }
        }
    },

    async asyncData({params}) {
        const response = await fetch(
            'https://gorest.co.in/public/v2/users/'+params.id
        ).then(res => res.json())

        return {
            user: response
        }
    }
}
</script>