<template>
  <!-- <v-
  <span>INi Adaladh ahalamdsakd</span> -->
  <!-- <button @click="logout">Logout</button> -->
  <v-container>
    <qrcode-stream
      v-if="isScanning"
      @decode="onDecode"
      :track="paintBoundingBox"
    >
    </qrcode-stream>
    <v-container v-else>
      <v-container
        v-if="isSending"
        class="d-flex justify-center align-center flex-column"
      >
        <v-progress-circular
          :size="70"
          :width="7"
          color="blue"
          indeterminate
        ></v-progress-circular>
        <p class="pt-2 pb-2 blue--text mt-5 font-weight-bold">Mengirim Data</p>
      </v-container>
      <v-container
        v-else
        class="d-flex justify-center align-center flex-column"
      >
        <v-avatar>
          <v-icon color="blue" size="50"> mdi-check-circle </v-icon>
        </v-avatar>
        <p class="pt-2 pb-2 blue--text mt-5 font-weight-bold">
          Data Telah Dikirim
        </p>
        <v-btn o depressed color="primary" @click="logout">
          <v-icon left dark> mdi-key </v-icon>
          Logout
        </v-btn>
      </v-container>
    </v-container>
  </v-container>
</template>

<script>
export default {
  async mounted() {
    const a = await this.$axios.$get('/user')
  },
  data() {
    return {
      isScanning: true,
      isSending: false,
    }
  },
  methods: {
    logout() {
      this.$auth.logout()
      this.$router.push('/')
    },
    async onDecode(qrcode) {
      const today = new Date()
      const validate =
        today.getFullYear() + '' + (today.getMonth() + 1) + '' + today.getDate()
      if (qrcode === validate) {
        this.isScanning = false
        this.isSending = true
        const absen = await this.$axios.$post('/absen', { qrcode })

    this.isSending = false
      } else {
        console.log(qrcode + ' ' + validate)
      }
    },
    paintBoundingBox(detectedCodes, ctx) {
      for (const detectedCode of detectedCodes) {
        const {
          boundingBox: { x, y, width, height },
        } = detectedCode

        ctx.lineWidth = 2
        ctx.strokeStyle = '#007bff'
        ctx.strokeRect(x, y, width, height)
      }
    },
  },
}
</script>

<style>
</style>
