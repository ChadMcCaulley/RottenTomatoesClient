<template>
  <v-dialog
    v-model="showDialog"
    width="500"
  >
    <template v-slot:activator="{ on }">
      <v-btn
        v-on="on"
        text
        class="mr-2"
      >
        Sign Up
      </v-btn>
    </template>

    <v-card>
      <v-card-title
        class="headline primary white--text"
        primary-title
      >
        Sign Up
        <v-spacer />
        <v-btn
          icon
          dark
          @click="showDialog = false"
        >
          <v-icon> mdi-close-circle </v-icon>
        </v-btn>
      </v-card-title>
      <ValidationObserver v-slot="{ invalid }">
        <v-form @submit.prevent="register">
          <v-container>
            <ValidationProvider
              rules="required"
              v-slot="{ errors }"
            >
              <v-text-field
                v-model="username"
                label="Username"
                prepend-inner-icon="mdi-account"
                :error-messages="errors"
              />
            </ValidationProvider>
            <ValidationProvider
              rules="required|email"
              v-slot="{ errors }"
            >
              <v-text-field
                v-model="email"
                label="Email"
                prepend-inner-icon="mdi-email"
                :error-messages="errors"
              />
            </ValidationProvider>
            <ValidationProvider
              rules="required"
              v-slot="{ errors }"
            >
              <v-text-field
                v-model="password"
                label="Password"
                :type="displayType"
                :append-icon="displayIcon"
                :error-messages="errors"
                @click:append="toggleIcon"
              />
            </ValidationProvider>
            <div class="mt-4 text-center">
              <v-btn
                class="dark"
                color="info"
                type="submit"
                :disabled="invalid"
              >
                Sign Up
              </v-btn>
            </div>
          </v-container>
        </v-form>
      </ValidationObserver>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'LogInDialog',
  data: () => ({
    showDialog: false,
    username: null,
    email: null,
    password: null,
    displayType: 'password',
    displayIcon: 'mdi-eye'
  }),
  methods: {
    /**
     * Change the icon and text display type of the password input
     */
    toggleIcon () {
      this.displayType = this.displayType === 'text' ? 'password' : 'text'
      this.displayIcon = this.displayIcon === 'mdi-eye' ? 'mdi-eye-off' : 'mdi-eye'
    },
    /**
     * Register a new user to the system
     */
    register () {
      const params = {
        username: this.username,
        password: this.password
      }
      this.$store.dispatch('auth/logIn', params)
    }
  }
}
</script>
