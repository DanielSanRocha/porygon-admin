<template>
  <v-container class="index-row">
    <form @submit="submit">
      <v-text-field v-model="id" label="Id" disabled />
      <v-text-field v-model="name" label="Name" />
      <v-text-field v-model="email" label="Email" />
      <v-btn type="submit">
        Edit
      </v-btn>
    </form>
  </v-container>
</template>

<script>
export default {
  name: 'UserPage',
  data: () => {
    return { id: null, name: '', email: '' }
  },
  mounted: function () {
    const that = this
    const urlParams = new URLSearchParams(window.location.search)
    this.id = urlParams.get('id')

    this.$axios.get(`/api/user/${this.id}`)
      .then((response) => {
        console.log(response)
        that.id = response.data.id
        that.name = response.data.name
        that.email = response.data.email
      }).catch((e) => {
        console.error(e)
        alert(e.response.data.message)
      })
  },
  methods: {
    submit: function (e) {
      e.preventDefault()
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
