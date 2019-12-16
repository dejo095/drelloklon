<template>
  <v-layout row justify-center>
    <v-flex xs4 mt-5>
      <h1 class="display-1">Register</h1>
      <v-progress-linear v-if="isCreating" :indeterminate="true"></v-progress-linear>
      <v-form v-if="!isCreating" ref="form" v-model="valid" lazy-validation>

        <v-text-field v-model="user.username" :rules="nameRules" label="Username" required></v-text-field>
        <v-text-field v-model="user.displayName" :rules="nameRules" label="Display name" required></v-text-field>
        <v-text-field v-model="user.password" :rules="passwordRules" label="Password" required type="password"></v-text-field>
        <v-text-field v-model="user.passwordConfirm" :rules="passwordConfirmRules" label="Confirm Password" required type="password"></v-text-field>
        <v-text-field v-model="user.imageUrl" label="Image Url" required></v-text-field>

        <v-btn :disabled="!valid" color="success" @click="register">
          Register
        </v-btn>

      </v-form>
    </v-flex>
  </v-layout>
</template>

<script>
import { mapState } from 'vuex'

export default {
    data: (vm) => ({
      valid: false,
      user: {
        username: '',
        password: '',
        passwordConfirm: '',
        displayName: '',
        imageUrl: '',
      },
      nameRules: [
        v => !!v || 'Cannot be Empty',
        v => (v && v.length <= 10) || 'Value must be less than 10 characters'
      ],
      emailRules: [
        v => !!v || 'Cannot be Empty',
        v => /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(v) || 'E-mail must be valid'
      ],
      passwordRules: [
        v => !!v || 'Cannot be Empty',
        v => /^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*[!@#\$%\^&\*])(?=.{8,})/.test(v) || 'Password doesnt comply with criteria'
      ],
      passwordConfirmRules: [
        v => vm.user.passwordConfirm === vm.user.password || 'Passwords do not match'
      ],
    }),
    computed: {
      ...mapState('users', { isCreating: 'isCreatePending' }),
    },
    methods: {
      register () {
        if (this.valid && (this.user.password === this.user.passwordConfirm)) {
          const { User } = this.$FeathersVuex
          const _user = new User(this.user)
          _user.save()
            .then(__user => {
              this.$router.push('/login')
            })
        } else {
          throw new Error('Error')
        }
      }
    }
  }
</script>

<style>

</style>
