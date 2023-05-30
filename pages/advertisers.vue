<template>
  <v-container>
    <v-table class="advertisers-table">
      <thead>
        <tr>
          <th class="text-left">
            Name
          </th>
          <th class="text-left">
            Description
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="advertiser in advertisers"
          :key="advertiser.id"
        >
          <td><a :href="generateLink(advertiser)">{{ advertiser.name }}</a></td>
          <td>{{ advertiser.description }}</td>
        </tr>
      </tbody>
    </v-table>
    <v-btn href="/advertiser">
      Create
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'AdvertisersPage',
  data: () => ({
    advertisers: []
  }),
  mounted: function () {
    const that = this
    this.$axios.get('/api/advertisers?limit=100&offset=0')
      .then((response) => {
        that.advertisers = response.data.data
      }).catch((e) => {
        console.error(e)
        alert(e.response.data.message)
      })
  },
  methods: {
    generateLink: function (advertiser) {
      return `/advertiser?id=${advertiser.id}`
    }
  }
}
</script>

<style lang="scss" scoped>
.advertisers-table th,td {
    border: 1px solid;
}
</style>
