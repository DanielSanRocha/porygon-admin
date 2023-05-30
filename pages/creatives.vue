<template>
  <v-container>
    <v-table class="creatives-table">
      <thead>
        <tr>
          <th class="text-left">
            ID
          </th>
          <th class="text-left">
            Name
          </th>
          <th class="text-left">
            Advertiser
          </th>
          <th class="text-left">
            Filename
          </th>
          <th class="text-left">
            Description
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="creative in creatives"
          :key="creative.id"
        >
          <td>{{ creative.id }}</td>
          <td><a :href="generateLink(creative)">{{ creative.name }}</a></td>
          <td>
            <a
              target="_blank"
              :href="'/advertiser?id=' + creative.id_advertiser.id"
            >{{ typeof(creative.id_advertiser) === 'object' ? creative.id_advertiser.name : creative.id_advertiser }}</a>
          </td>
          <td>{{ creative.filename }}</td>
          <td>{{ creative.description }}</td>
        </tr>
      </tbody>
    </v-table>
    <v-btn class="create-creative-button" href="/creative">
      Create
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CreativesPage',
  data: () => ({
    creatives: []
  }),
  watch: {
    creatives (newCreatives, _) {
      newCreatives.forEach((creative, index) => {
        const that = this
        this.$axios.get(`/api/advertiser/${creative.id_advertiser}`)
          .then((response) => {
            that.creatives[index].id_advertiser = response.data
          }).catch((e) => {
            console.error(e)
            alert(e.response.data.message)
          })
      })
    }
  },
  mounted: function () {
    const that = this
    this.$axios.get('/api/creatives?limit=100&offset=0')
      .then((response) => {
        that.creatives = response.data.data
      }).catch((e) => {
        console.error(e)
        alert(e.response.data.message)
      })
  },
  methods: {
    generateLink: function (creative) {
      return `/creative?id=${creative.id}`
    }
  }
}
</script>

<style lang="scss" scoped>
.creatives-table th,td {
    border: 1px solid;
    padding: 5px;
}

.creatives-table {
    width: 100%;
    display: block;
}

.create-creative-button {
    margin-top: 30px;
}
</style>
