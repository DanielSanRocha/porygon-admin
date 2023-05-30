<template>
  <v-container class="index-row">
    <v-card width="600">
      <v-card-item>
        <v-card-title>Server Status</v-card-title>
        <v-card-subtitle>Healthcheck Response</v-card-subtitle>
      </v-card-item>
      <v-card-text :class="{green: isOk, red: !isOk}">
        Status: <b>{{ status }}</b>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'IndexPage',
  data: () => {
    return { status: '', isOk: true }
  },
  mounted: function () {
    const that = this
    this.$axios.get('/healthcheck')
      .then((response) => {
        that.status = response.data.message
        that.isOk = true
      }).catch((e) => {
        console.error(e)
        that.isOk = false
        that.status = e.response ? e.response.data.message : 'Connection Refused'
      })
  }
}</script>

<style lang="scss" scoped>
.index-row {
  padding: 50px;
}

.green {
  color: green;
}

.red {
  color: red;
}
</style>
