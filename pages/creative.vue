<template>
  <v-container class="index-row">
    <form @submit="submit">
      <v-text-field v-model="id" label="Id" disabled />
      <v-combobox
        v-model="advertiser"
        label="advertiser"
        :items="advertisers"
        variant="outlined"
        item-text="name"
        prepend-icon="mdi-domain"
        single-line
        return-object
      />
      <v-text-field v-model="name" label="Name" />
      <v-textarea v-model="description" label="Description" />
      <v-file-input label="image" type="file" @change="onFileSelected" />
      <v-text-field v-model="filename" label="Filename" disabled />
      <v-text-field v-model="width" label="Width" disabled />
      <v-text-field v-model="height" label="Height" disabled />
      <v-btn type="submit">
        {{ mode === 'create' ? 'Create' : 'Edit' }}
      </v-btn>
      <v-btn v-if="mode === 'edit'" color="red" type="button" @click="deleteCreative">
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
  name: 'CreativePage',
  data: () => {
    return { advertisers: [], id: null, advertiser: null, name: '', filename: '', description: '', width: 0, height: 0, mode: 'create', downloadLink: '' }
  },
  mounted: function () {
    const that = this
    const urlParams = new URLSearchParams(window.location.search)
    this.id = urlParams.get('id')

    this.$axios.get('/api/advertisers?limit=10000&offset=0')
      .then((response) => {
        console.log(response)
        that.advertisers = response.data.data
      }).catch((e) => {
        console.log(e)
        alert(e.response.data.message)
      })

    if (this.id) {
      this.mode = 'edit'
      this.$axios.get(`/api/creative/${this.id}`)
        .then((response) => {
          console.log(response)
          that.id = response.data.id
          that.id_advertiser = response.data.id_advertiser
          that.$axios.get(`/api/advertiser/${that.id_advertiser}`)
            .then((response) => {
              that.advertiser = response.data
            }).catch((e) => {
              console.error(e)
              alert(e.response.data.message)
            })
          that.name = response.data.name
          that.filename = response.data.filename
          that.downloadLink = `${response.config.baseURL}/download/${response.data.filename}`
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

      if (this.filename.length < 3) {
        alert('You must upload a image first')
        return
      }

      if (this.advertiser === null) {
        alert('Advertiser must be set')
        return
      }

      if (this.mode === 'create') {
        this.$axios.post('/api/creative',
          { id_advertiser: this.advertiser.id, name: this.name, description: this.description, filename: this.filename, width: this.width, height: this.height })
          .then((response) => {
            console.log(response)
            alert('created!')
            that.$router.push(`/creative?id=${response.data.id}`)
            that.id = response.data.id
            that.mode = 'edit'
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      } else if (this.mode === 'edit') {
        this.$axios.put(`/api/creative/${this.id}`,
          { id_advertiser: this.advertiser.id, name: this.name, description: this.description, filename: this.filename, width: this.width, height: this.height })
          .then((response) => {
            console.log(response)
            alert('updated!')
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      }
    },
    deleteCreative: function (e) {
      e.preventDefault()
      const that = this

      const result = window.confirm('Are you sure you want to delete this creative?')

      if (result) {
        this.$axios.delete(`/api/creative/${this.id}`)
          .then((response) => {
            console.log(response)
            alert('deleted')
            that.$router.push('/creatives')
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      }
    },
    onFileSelected: function (f) {
      const that = this
      const fd = new FormData()
      fd.append('file', f, f.name)
      this.$axios.post('/upload', fd)
        .then((response) => {
          console.log(response)
          alert(`Uploaded with success. Filename: ${response.data.filename}`)
          that.filename = response.data.filename
          that.downloadLink = `${response.config.baseURL}/download/${response.data.filename}`
          that.width = response.data.width
          that.height = response.data.height
        }).catch((e) => {
          console.error(e)
          that.filename = ''
          that.downloadLink = ''
          that.width = 0
          that.height = 0
          alert(e.response.data.message)
        })
    }
  }
}
</script>

<style lang="scss" scoped>
</style>
