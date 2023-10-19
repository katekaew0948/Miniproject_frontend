<template>
  <v-card>
    <v-card-text>
      <v-card-title style="font-size: 24 px">เข้าสู่ระบบ</v-card-title>
      <v-form ref="loginForm" v-model="valid" lazy-validation>
        <v-text-field
          v-model="username"
          :rules="nameRules"
          label="ชื่อผู้ใช้"
          placeholder="ชื่อผู้ใช้"
          required
          outlined
        ></v-text-field>
        <v-text-field
          v-model="password"
          :rules="passwordRules"
          label="รหัสผ่าน"
          placeholder="รหัสผ่าน"
          required
          outlined
        ></v-text-field>
        <v-alert v-if="response" border="left" dense text  type="error"> {{response }}</v-alert>
        <v-btn
          :disabled="!valid"
          color="success"
          class="mr-4"
          @click="Login()"
          block
        >
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
    username: "",
    response: "",
    nameRules: [(v) => !!v || "กรุณากรอกชื่อผู้ใช้งาน"],
    password: "",
    passwordRules: [(v) => !!v || "กรุณากรอกรหัสผ่าน"],
    accId: 0,
    account: [],
  }),
  created () {
    if(localStorage.getItem("loginStatus") == 'true'){
      this.$router.push("/");
    }
  },

  methods: {
    async Login() {
      try {
        if (this.$refs.loginForm.validate(true)) {
          var checkAccount = await this.axios.get(
            "http://localhost:9000/account?username=" +
              this.username +
              "&password=" +
              this.password
          );
          console.log("checkAcc =>", checkAccount);
          this.account = checkAccount.data;
          console.log("my Account =>", this.account);
          this.accId = this.account.id;
          console.log("AccId => ", this.accId);
          localStorage.setItem("username", this.username);
          localStorage.setItem("loginStatus" , true);
          localStorage.setItem("accId", this.accId);
          this.$EventBus.$emit("getUsername", this.username);
          this.$router.push("/content/" + this.accId);
        }
      } catch (error) {
        console.log(error.response.data);
        this.response = error.response.data;
        // localStorage.setItem("username", '');
      }
    },
  },
};
</script>

<style>
</style>