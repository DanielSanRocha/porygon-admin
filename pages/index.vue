<template>
  <v-container class="index-row">
    <v-card width="600">
      <v-card-item>
        <v-card-title>Server Status</v-card-title>
        <v-card-subtitle>Healthcheck Response</v-card-subtitle>
      </v-card-item>
      <v-card-text :class="{green: isOk}">
        Status: {{ status }}
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'IndexPage',
  data: () => {
    return { status: 'Ok', isOk: true }
  }
}</script>

<style lang="scss" scoped>
.index-row {
  padding: 50px;
}

.green {
  color: green;
}
</style>
