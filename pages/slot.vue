<template>
  <v-container class="index-row">
    <form @submit="submit">
      <v-text-field v-model="id" label="Id" disabled />
      <v-text-field v-model="name" label="Name" />
      <v-textarea v-model="description" label="Description" />
      <v-text-field v-model="width" label="Width" />
      <v-text-field v-model="height" label="Height" />
      <v-btn type="submit">
        {{ mode === 'create' ? 'Create' : 'Edit' }}
      </v-btn>
      <v-btn v-if="mode === 'edit'" color="red" type="button" @click="deleteSlot">
        Delete
      </v-btn>
      <v-btn v-if="filename" type="button" target="_blank" :href="downloadLink" color="blue">
        Download
      </v-btn>
    </form>
  </v-container>
</template>

<script>
export default {
  name: 'SlotPage',
  data: () => {
    return { id: null, name: '', description: '', width: 0, height: 0, mode: 'create' }
  },
  mounted: function () {
    const that = this
    const urlParams = new URLSearchParams(window.location.search)
    this.id = urlParams.get('id')

    if (this.id) {
      this.mode = 'edit'
      this.$axios.get(`/api/slot/${this.id}`)
        .then((response) => {
          console.log(response)
          that.id = response.data.id
          that.name = response.data.name
          that.description = response.data.description
          that.width = response.data.width
          that.height = response.data.height
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

      if (this.name.length < 3) {
        alert('Name must be at least 3 characters long')
        return
      }

      if (!this.width) {
        alert('You must set a width')
        return
      }

      if (!this.height) {
        alert('You must set a height')
        return
      }

      if (this.mode === 'create') {
        this.$axios.post('/api/slot',
          { name: this.name, description: this.description, width: this.width, height: this.height })
          .then((response) => {
            console.log(response)
            alert('created!')
            that.$router.push(`/slot?id=${response.data.id}`)
            that.id = response.data.id
            that.mode = 'edit'
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      } else if (this.mode === 'edit') {
        this.$axios.put(`/api/slot/${this.id}`,
          { name: this.name, description: this.description, width: this.width, height: this.height })
          .then((response) => {
            console.log(response)
            alert('updated!')
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      }
    },
    deleteSlot: function (e) {
      e.preventDefault()
      const that = this

      const result = window.confirm('Are you sure you want to delete this slot?')

      if (result) {
        this.$axios.delete(`/api/slot/${this.id}`)
          .then((response) => {
            console.log(response)
            alert('deleted')
            that.$router.push('/slots')
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
