<template>
  <v-container />
</template>

<script>
export default {
  name: 'LogoutPage',
  mounted: function () {
    localStorage.removeItem('token')
    this.$router.push('/login')
  }
}</script>

<style lang="scss" scoped>
</style>
