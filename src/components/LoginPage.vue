<template>
  <v-card>
    <v-card-title style="font-size: 24px; important!;">เข้าสู่ระบบ</v-card-title>
    <v-card-title> PSU passport </v-card-title>
    <v-card-text>
      <v-form ref="LoginForm" v-model="valid" lazy-validation>
        <v-text-field
          v-model="email"
          :counter="20"
          :rules="nameRules"
          label="ชื่อผู้ใช้งาน"
          required
          outlined
        ></v-text-field>

        <v-text-field
          v-model="password"
          :type="showPassword ? 'text' : 'password'"
          :rules="passwordRules"
          label="รหัสผ่าน"
          required
          outlined
        ></v-text-field>
        <v-btn :disabled="!valid" color="success" @click="performLogin" block>
          เข้าสู่ระบบ
        </v-btn>
      </v-form>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    // users: [
    //   { username: '6310210449@psu.ac.th', password: 'water449.' },
    //   { username: 'user2', password: 'password2' }
    //   // เพิ่มผู้ใช้และรหัสผ่านเพิ่มเติมตามต้องการ
    // ],
    email: '',
    nameRules: [
      v => !!v || 'กรุณากรอกชื่อผู้ใช้งาน',
      v => (v && v.length <= 20) || 'กรุณากรอกข้อมูลชื่อผู้ใช้งานห้ามเกิน 20 ตัวอักษร'
    ],
    password: '',
    passwordRules: [v => !!v || 'กรุณากรอกรหัสผ่าน'],
    showPassword: false
  }),

  methods: {
    async performLogin () {
      this.isLoggingIn = true
      try {
        const response = await this.axios.post(
          'http://localhost:8081/login',
          {
            email: this.email,
            password: this.password
          }
        )

        if (response.status === 200) {
          // Login successful
          // this.userAvatar = response.data.avatar
          localStorage.setItem('auth', JSON.stringify(response.data))
          const auth = JSON.parse(localStorage.getItem('auth'))
          console.log(auth)

          this.$router
            .push({ path: '/Homeview' })
            .then(() => {
              window.location.reload()
            })
            .catch(() => {})

          console.log('Login successful')
        } else {
          // Handle other response statuses (e.g., 401 for unauthorized)
          console.log('Login failed')
        }
      } catch (error) {
        // Handle any network or server errors
        console.error('Error:', error.message)
      } finally {
        this.isLoggingIn = false
      }
    }
  }

}
</script>

<style>
</style>
