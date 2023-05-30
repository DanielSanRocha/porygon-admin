<template>
  <v-container class="index-row">
    <form @submit="submit">
      <v-text-field v-model="id" label="Id" disabled />
      <v-text-field v-model="name" label="Name" />
      <v-textarea v-model="description" label="Description" />
      <v-btn type="submit">
        {{ mode === 'create' ? 'Create' : 'Edit' }}
      </v-btn>
      <v-btn color="red" type="button" @click="deleteAdvertiser">
        Delete
      </v-btn>
    </form>
  </v-container>
</template>

<script>
export default {
  name: 'AdvertiserPage',
  data: () => {
    return { id: null, name: '', description: '', mode: 'create' }
  },
  mounted: function () {
    const that = this
    const urlParams = new URLSearchParams(window.location.search)
    this.id = urlParams.get('id')

    if (this.id) {
      this.mode = 'edit'
      this.$axios.get(`/api/advertiser/${this.id}`)
        .then((response) => {
          console.log(response)
          that.id = response.data.id
          that.name = response.data.name
          that.description = response.data.description
        }).catch((e) => {
          console.error(e)
          alert(e.response.data.message)
        })
    } else {
      this.mode = 'create'
    }
  },
  methods: {
    submit: function (e) {
      e.preventDefault()
      const that = this

      if (this.mode === 'create') {
        this.$axios.post('/api/advertiser', { name: this.name, description: this.description })
          .then((response) => {
            console.log(response)
            alert('created!')
            that.$router.push(`/advertiser?id=${response.data.id}`)
            that.id = response.data.id
            that.mode = 'edit'
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      } else if (this.mode === 'edit') {
        this.$axios.put(`/api/advertiser/${this.id}`, { name: this.name, description: this.description })
          .then((response) => {
            console.log(response)
            alert('updated!')
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      }
    },
    deleteAdvertiser: function (e) {
      e.preventDefault()
      const that = this

      const result = window.confirm('Are you sure you want to delete this advertiser?')

      if (result) {
        this.$axios.delete(`/api/advertiser/${this.id}`)
          .then((response) => {
            console.log(response)
            alert('deleted')
            that.$router.push('/advertisers')
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
