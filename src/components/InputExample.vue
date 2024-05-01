<template>
  <div>
    <v-row>
      <v-col cols="6">
        <!-- login -->

        <div>
          <v-img class="mx-auto my-6" max-width="228"
            src="https://cdn.vuetifyjs.com/docs/images/logos/vuetify-logo-v3-slim-text-light.svg"></v-img>

          <v-card class="mx-auto pa-12 pb-8" elevation="8" max-width="448" rounded="lg">
            <div class="text-subtitle-1 text-medium-emphasis">Account</div>

            <v-text-field density="compact" placeholder="Email address" prepend-inner-icon="mdi-email-outline"
              variant="outlined"></v-text-field>

            <div class="text-subtitle-1 text-medium-emphasis d-flex align-center justify-space-between">
              Password

              <a class="text-caption text-decoration-none text-blue" href="#" rel="noopener noreferrer" target="_blank">
                Forgot login password?</a>
            </div>

            <v-text-field :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'" :type="visible ? 'text' : 'password'"
              density="compact" placeholder="Enter your password" prepend-inner-icon="mdi-lock-outline"
              variant="outlined" @click:append-inner="visible = !visible"></v-text-field>

            <v-card class="mb-12" color="surface-variant" variant="tonal">
              <v-card-text class="text-medium-emphasis text-caption">
                Warning: After 3 consecutive failed login attempts, you account will be temporarily locked for three
                hours. If you must login now, you can also click "Forgot login password?" below to reset the login
                password.
              </v-card-text>
            </v-card>

            <v-btn class="mb-8" color="blue" size="large" variant="tonal" block>
              Log In
            </v-btn>

            <v-card-text class="text-center">
              <a class="text-blue text-decoration-none" href="#" rel="noopener noreferrer" target="_blank">
                Sign up now <v-icon icon="mdi-chevron-right"></v-icon>
              </a>
            </v-card-text>
          </v-card>
        </div>

      </v-col>
      <v-col cols="6">

        <v-row justify="center">
          <v-col cols="12" lg="6" md="8" sm="10">
            <v-card ref="form">
              <v-card-text>
                <v-text-field ref="name" v-model="name" :error-messages="errorMessages"
                  :rules="[() => !!name || 'This field is required']" label="Full Name" placeholder="John Doe"
                  required></v-text-field>
                <v-text-field ref="address" v-model="address" :rules="[
              () => !!address || 'This field is required',
              () => !!address && address.length <= 25 || 'Address must be less than 25 characters',
              addressCheck
            ]" counter="25" label="Address Line" placeholder="Snowy Rock Pl" required></v-text-field>
                <v-text-field ref="city" v-model="city"
                  :rules="[() => !!city || 'This field is required', addressCheck]" label="City" placeholder="El Paso"
                  required></v-text-field>
                <v-text-field ref="state" v-model="state" :rules="[() => !!state || 'This field is required']"
                  label="State/Province/Region" placeholder="TX" required></v-text-field>
                <v-text-field ref="zip" v-model="zip" :rules="[() => !!zip || 'This field is required']"
                  label="ZIP / Postal Code" placeholder="79938" required></v-text-field>
                <v-autocomplete ref="country" v-model="country" :items="countries"
                  :rules="[() => !!country || 'This field is required']" label="Country" placeholder="Select..."
                  required></v-autocomplete>
              </v-card-text>
              <v-divider class="mt-12"></v-divider>
              <v-card-actions>
                <v-btn variant="text">
                  Cancel
                </v-btn>
                <v-spacer></v-spacer>
                <v-slide-x-reverse-transition>
                  <v-tooltip v-if="formHasErrors" location="left">
                    <template v-slot:activator="{ on, attrs }">
                      <v-btn class="my-0" icon v-bind="attrs" v-on="on" @click="resetForm">
                        <v-icon>mdi-refresh</v-icon>
                      </v-btn>
                    </template>
                    <span>Refresh form</span>
                  </v-tooltip>
                </v-slide-x-reverse-transition>
                <v-btn color="primary" variant="text" @click="submit">
                  Submit
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>

      </v-col>

      <v-col cols="6">

        <v-sheet class="bg-deep-purple pa-12" rounded>
          <v-card class="mx-auto px-6 py-8" max-width="344">
            <v-form v-model="form2" @submit.prevent="onSubmit">
              <v-text-field v-model="email" :readonly="loading" :rules="[required]" class="mb-2" label="Email"
                clearable></v-text-field>

              <v-text-field v-model="password" :readonly="loading" :rules="[required]" label="Password"
                placeholder="Enter your password" clearable></v-text-field>

              <br>

              <v-btn :disabled="!form2" :loading="loading" color="success" size="large" type="submit" variant="elevated"
                block>
                Sign In
              </v-btn>
            </v-form>
          </v-card>
        </v-sheet>


      </v-col>
      <v-col cols="6">
        <v-container fluid>
          <p>Selected Button: {{ radios }}</p>
          <v-radio-group v-model="radios">
            <v-radio label="Option One" value="one"></v-radio>
            <v-radio label="Option 2 (string)" value="2"></v-radio>
            <v-radio :value="3" label="Option 3 (integer)"></v-radio>
          </v-radio-group>
        </v-container>

      </v-col>
      <v-col cols="6"> hi </v-col>
    </v-row>
  </div>
</template>

<script setup>
const items = [
  {
    title: 'Vuetify Documentation',
    icon: `$vuetify`,
    href: 'https://vuetifyjs.com/',
  },
  {
    title: 'Vuetify Support',
    icon: 'mdi-shield-star-outline',
    href: 'https://support.vuetifyjs.com/',
  },
  {
    title: 'Vuetify X',
    icon: `svg:M2.04875 3.00002L9.77052 13.3248L1.99998 21.7192H3.74882L10.5519 14.3697L16.0486 21.7192H22L13.8437 10.8137L21.0765 3.00002H19.3277L13.0624 9.76874L8.0001 3.00002H2.04875ZM4.62054 4.28821H7.35461L19.4278 20.4308H16.6937L4.62054 4.28821Z`,
    href: 'https://x.com/vuetifyjs',
  },
  {
    title: 'Vuetify GitHub',
    icon: `mdi-github`,
    href: 'https://github.com/vuetifyjs/vuetify',
  },
  {
    title: 'Vuetify Discord',
    icon: `mdi-discord`,
    href: 'https://community.vuetifyjs.com/',
  },
  {
    title: 'Vuetify Reddit',
    icon: `mdi-reddit`,
    href: 'https://reddit.com/r/vuetifyjs',
  },
]
</script>

<script>
export default {
  data: () => ({
    visible: false,
    countries: ['Afghanistan', 'Albania', 'Algeria', 'Andorra', 'Angola', 'Anguilla', 'Antigua &amp; Barbuda', 'Argentina', 'Armenia'],
    errorMessages: '',
    name: null,
    address: null,
    city: null,
    state: null,
    zip: null,
    country: null,
    formHasErrors: false,

    form2: false,
    email: null,
    password: null,
    loading: false,

    radios: 'one',

  }),

  computed: {
    form() {
      return {
        name: this.name,
        address: this.address,
        city: this.city,
        state: this.state,
        zip: this.zip,
        country: this.country,
      }
    },
  },

  watch: {
    name() {
      this.errorMessages = ''
    },
  },

  methods: {
    addressCheck() {
      this.errorMessages = this.address && !this.name
        ? `Hey! I'm required`
        : ''

      return true
    },
    resetForm() {
      this.errorMessages = []
      this.formHasErrors = false

      Object.keys(this.form).forEach(f => {
        this.$refs[f].reset()
      })
    },
    submit() {
      this.formHasErrors = false

      Object.keys(this.form).forEach(f => {
        if (!this.form[f]) this.formHasErrors = true

        this.$refs[f].validate(true)
      })
    },

    onSubmit() {
      if (!this.form2) return

      this.loading = true

      setTimeout(() => (this.loading = false), 2000)
    },
    required(v) {
      return !!v || 'Field is required'
    },

  },

}
</script>

<style scoped lang="sass">
  .social-link :deep(.v-icon)
    color: rgba(var(--v-theme-on-background), var(--v-disabled-opacity))
    text-decoration: none
    transition: .2s ease-in-out

    &:hover
      color: rgba(25, 118, 210, 1)
</style>
