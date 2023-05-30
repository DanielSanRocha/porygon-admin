<template>
  <v-container>
    <v-table class="slots-table">
      <thead>
        <tr>
          <th class="text-left">
            ID
          </th>
          <th class="text-left">
            Name
          </th>
          <th class="text-left">
            Width
          </th>
          <th class="text-left">
            Height
          </th>
          <th class="text-left">
            Description
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="slot in slots"
          :key="slot.id"
        >
          <td>{{ slot.id }}</td>
          <td><a :href="generateLink(slot)">{{ slot.name }}</a></td>
          <td>{{ slot.width }}</td>
          <td>{{ slot.height }}</td>
          <td>{{ slot.description }}</td>
        </tr>
      </tbody>
    </v-table>
    <v-btn class="create-slot-button" href="/slot">
      Create
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'SlotsPage',
  data: () => ({
    slots: []
  }),
  mounted: function () {
    const that = this
    this.$axios.get('/api/slots?limit=100&offset=0')
      .then((response) => {
        that.slots = response.data.data
      }).catch((e) => {
        console.error(e)
        alert(e.response.data.message)
      })
  },
  methods: {
    generateLink: function (slot) {
      return `/slot?id=${slot.id}`
    }
  }
}
</script>

<style lang="scss" scoped>
.slots-table th,td {
    border: 1px solid;
    padding: 5px;
}

.slots-table {
    width: 100%;
    display: block;
}

.create-slot-button {
    margin-top: 30px;
}
</style>
