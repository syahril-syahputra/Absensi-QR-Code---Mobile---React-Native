<template>
  <!-- <v-
  <span>INi Adaladh ahalamdsakd</span> -->
  <button @click="logout">Logout</button>
</template>

<script>
export default {
  async mounted() {
    const a = await this.$axios.$get('/user')
    console.log(a)
  },
  methods: {
    logout() {
      this.$auth.logout()
      this.$router.push('/')
    },
  },
}
</script>

<style>
</style>
