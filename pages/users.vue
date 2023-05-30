<template>
  <v-container class="users-container">
    <v-text-field label="Search..." variant="outlined" class="users-search" />
    <v-row v-for="user in users" :key="user.email">
      <v-card width="900" class="user-card">
        <v-card-item>
          <v-card-title> {{ user.name }}</v-card-title>
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
    users: [
      { name: 'Daniel', email: 'danielsantanarocha@gmail.com', updatedAt: '2022', createdAt: '2022' },
      { name: 'Daniel', email: 'danielsantanarocha@gmail.com', updatedAt: '2022', createdAt: '2022' },
      { name: 'Daniel', email: 'danielsantanarocha@gmail.com', updatedAt: '2022', createdAt: '2022' }
    ]
  })
}</script>

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
