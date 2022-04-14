<template>
  <v-form class="ma-4">
    <p class="pt-2 pb-2 gray--text font-weight-bold">Silahkan Login</p>
    <v-text-field
      label="Username"
      clearable
      v-model="username"
      prepend-icon="mdi-account"
      hint="Masukan Username Anda"
    ></v-text-field>
    <v-text-field
      prepend-icon="mdi-lock"
      v-model="password"
      :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
      :type="show1 ? 'text' : 'password'"
      name="input-10-1"
      label="Password"
      hint="Masukan Password Anda"
      @click:append="show1 = !show1"
      clearable
    ></v-text-field>
    <v-container class="d-flex justify-space-between align-center">
      <span class="forgetpassword" @click="forget">Forget Password ? </span>
      <v-btn o depressed color="primary" @click="login">
        <v-icon left dark> mdi-key </v-icon>
        Login
      </v-btn>
    </v-container>
    <v-snackbar>Username Atau Password Salah</v-snackbar>
  </v-form>
</template>

<script>
import { AxiosRequestConfig } from 'axios'
export default {
  name: 'IndexPage',
  data() {
    return {
      // username: '11684',
      // password: '11684',
      username: '',
      password: '',
      show1: false,
      status:"",
      snackbar: false,
    }
  },
  async mounted(){
      this.status = 'FETCHING';
      const vm = this;
      navigator.mediaDevices.getUserMedia({
        video: true,
        audio: false,
      }).then(() => {
        this.status = 'DONE FETCHING';
      }, (err) => {
        console.error(err);
      });

  },
  methods: {
    async login() {
      if (this.username === '' || this.password === '') {
        alert('Masukan Email/Username dan Password Anda')
      } else {
        await this.$auth
          .login({
            data: {
              username: this.username,
              password: this.password,
            },
          })
          .then(() => {
            this.$router.push('/home')
          })
          .catch((err) => {
            alert('Username Atau Password Anda Salah')
            this.snackbar = true
            console.log(err)
          })
      }
    },
    forget() {
      alert('Silahkan Hubungi Admin Untuk Mendapatkan Password Login')
    },
  },
}
</script>
<style scoped>
.v-form {
  background-color: #f9f9f9;
  padding: 10px;
  border-radius: 10px;
  margin: 0px auto;
  box-shadow: 2px 2px 2px #cccccc;
}
.forgetpassword {
  color: #777777;
  font-weight: bold;
}
</style>
