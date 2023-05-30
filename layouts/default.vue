<template>
  <v-app>
    <v-navigation-drawer app class="sidebar">
      <v-list-item class="topside-sidebar">
        <v-list-item-content>
          <v-list-item-title class="text-h6">
            Porygon
          </v-list-item-title>
          <v-list-item-subtitle>Administration Panel</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
      <v-divider />
      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" :href="item.link">
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="main-container">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data () {
    return {
      items: [
        { title: 'Home', icon: 'mdi-view-dashboard', link: '/' },
        { title: 'Advertisers', icon: 'mdi-domain', link: '/advertisers' },
        { title: 'Creatives', icon: 'mdi-creative-commons', link: '/creatives' },
        { title: 'Slots', icon: 'mdi-rectangle', link: '/slots' },
        { title: 'Campaigns', icon: 'mdi-newspaper', link: '/campaigns' },
        { title: 'Users', icon: 'mdi-account', link: '/users' },
        { title: 'Settings', icon: 'mdi-cog', link: '/settings' },
        { title: 'Logout', icon: 'mdi-logout', link: '/logout' }
      ]
    }
  },
  created: function () {
    const token = process.browser ? localStorage.getItem('token') : ''

    if (!token) {
      this.$router.push('/login')
    }
    this.$axios.onRequest((config) => {
      config.headers.common.Authorization = token
    })
  }
}
</script>

<style lang="scss" scoped>
.topside-sidebar {
  background-color: $dark-pink;
}

.sidebar {
  background-color: $light-pink;
}

.main-container {
  background-color: $light-blue;
}

.toolbar {
  height: 67px !important;
}
</style> scoped>
