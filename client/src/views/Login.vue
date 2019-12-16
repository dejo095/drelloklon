<template>
 <v-layout row justify-center>
    <v-flex xs12 sm-6 mt-5>
      <h1 class="display-1">Login</h1>
        <v-form ref="form" v-model="valid" lazy-validation>

          <v-text-field v-model="user.username" :rules="notEmptyRules" label="Username" required></v-text-field>

          <v-text-field v-model="user.password" :rules="notEmptyRules" label="Password" required></v-text-field>

          <v-btn :disabled="!valid" color="success" @click="login">
            Login
          </v-btn>

        </v-form>
    </v-flex>
 </v-layout>
</template>

<script>
export default {
    data: () => ({
      valid: true,
      user: {
        username: '',
        password: '',
      },
      notEmptyRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 1) || 'Name must be more then 1 character'
      ],
    }),

    methods: {
      login () {
        if (this.$refs.form.validate()) {

        }
      }
    }
  }
</script>

<style>

</style>
