<template>
  <v-row>
    <v-col cols="12" sm="4" />
    <v-col cols="12" sm="4">
      <v-card max-width="600" variant="outlined" class="login-card">
        <form @submit="submit">
          <v-card-title>Login</v-card-title>
          <v-text-field v-model="email" label="Email" variant="outlined" class="login-text-field" type="email" />
          <v-text-field v-model="password" label="Password" variant="outlined" class="login-text-field" type="password" />
          <v-btn variant="tonal" class="login-button" type="submit">
            Confirm
          </v-btn>
        </form>
      </v-card>
    </v-col>
    <v-col cols="12" sm="4" />
  </v-row>
</template>

<script>
export default {
  name: 'LoginPage',
  layout: 'empty',
  data: () => {
    return { email: '', password: '' }
  },
  mounted: function () {
    const token = localStorage.getItem('token')
    if (token) {
      this.$router.push('/')
    }
  },
  methods: {
    submit: function (e) {
      e.preventDefault()
      this.$axios.post('/api/login', { email: this.email, password: this.password })
        .then((response) => {
          console.log(response)
          localStorage.setItem('token', response.data.token)
          this.$router.push('/')
        })
        .catch((e) => {
          console.error(e)
          alert(e.response.data.message)
        })
    }
  }
}</script>

<style lang="scss" scoped>
.login-text-field {
    margin: 5px 10px 5px 10px;
    padding: 5px 5px 5px 5px;
}

.login-card {
    background-color: $light-pink !important;
}
.login-button {
    margin: 5px 10px 5px 10px;
    padding: 5px 5px 5px 5px;
    background-color: $dark-pink !important;
}
</style>
