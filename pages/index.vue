<template>
  <div>
    <b-container>
      <b-row class="vh-100 text-center" align-v="center">
        <b-col>
          <h2>Hello!</h2>
          <h2>Welcome to Users App with Nuxt.JS</h2>
          <nuxt-link :to="{ name: 'users' }"
            ><h5>Click here to access Users Data</h5></nuxt-link
          >
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'IndexPage',
})
</script>

<style>
div {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
</style>
