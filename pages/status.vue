<template>
  <v-container>
    <v-container v-if="isLoading">
      <v-row>
        <v-col cols="12" md="4">
          <v-skeleton-loader
            type="list-item-avatar, divider, list-item-two-line, list-item-two-line, list-item-two-line, actions"
          ></v-skeleton-loader>
        </v-col>
      </v-row>
    </v-container>
    <v-container v-else>
      <v-card class="mx-auto" max-width="400" tile>
        <v-container class="d-flex align-center">
          <v-avatar color="primary" size="56">
            <v-icon dark> mdi-account </v-icon>
          </v-avatar>

          <span class="ml-3 font-weight-bold">PN : {{ pn }}</span>
        </v-container>

        <v-list-item two-line>
          <v-list-item-content>
            <v-list-item-title>Nama</v-list-item-title>
            <v-list-item-subtitle>{{ nama }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-list-item two-line>
          <v-list-item-content>
            <v-list-item-title>Department</v-list-item-title>
            <v-list-item-subtitle>{{ department }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-list-item two-line>
          <v-list-item-content>
            <v-list-item-title>Status Absensi</v-list-item-title>
            <v-list-item-subtitle>{{ status ? "Selesai" : "Belum Absen" }}</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>
        <v-divider></v-divider>
        <v-container class="d-flex justify-space-between align-center">

      <span @click="logout" class="gray--text font-weight-bold">Logout </span>
      <v-btn v-if="!status" o depressed color="primary" @click="absen" >
        <v-icon left dark> mdi-file-chart </v-icon>
        Absen Now
      </v-btn>
    </v-container>
      </v-card>
    </v-container>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      isLoading: true,
      nama: '',
      pn: '',
      department: '',
      status: false,
    }
  },
  async mounted(){
    const get = await this.$axios.$get('/user-mobile')
    this.nama = get["0"].nama
    this.pn = get["0"].pn
    this.department = get["0"].department.name
    this.status = get['status']
    this.isLoading = false
  },
  methods : {
    logout(){

      this.$auth.logout()
      this.$router.push('/')

    },
    absen(){
      this.$router.push("/home")
    }
  }
}
</script>

<style>
</style>
