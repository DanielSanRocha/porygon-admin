<template>
  <v-container class="users-container">
    <!-- <v-text-field label="Search..." variant="outlined" class="users-search" /> -->
    <v-row v-for="user in users" :key="user.email">
      <v-card width="900" class="user-card" :href="generateLink(user)">
        <v-card-item>
          <v-card-title> {{ user.id }} - {{ user.name }}</v-card-title>
          <v-card-subtitle> {{ user.email }}</v-card-subtitle>
        </v-card-item>
      </v-card>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'UsersPage',
  data: () => ({
    users: []
  }),
  mounted: function () {
    const that = this
    this.$axios.get('/api/users?limit=100&offset=0')
      .then((response) => {
        that.users = response.data.data
      }).catch((e) => {
        console.error(e)
        alert(e.response.data.message)
      })
  },
  methods: {
    generateLink: function (user) {
      return `/user?id=${user.id}`
    }
  }
}
</script>

<style lang="scss" scoped>
.user-card {
    margin-top: 20px;
    margin-left: 0px;
}

.users-container {
    margin-left: 50px;
    margin-top: 30px;
}

.users-search {
    max-width: 30%;
}

</style>
