<template>
  <v-app>
    <v-app-bar
      app
      color="#F0CF6D"
      light
    >
      <div class="d-flex align-center red--text font-weight-bold">
        COMART
      </div>

      <v-spacer></v-spacer>

      <v-btn
        text
        @click="goToHome()"
      >
        หน้าหลัก
      </v-btn>

      <v-btn
        text
        @click="goToContent()"
      >
        หน้าจัดการร้าน
      </v-btn>

      <v-btn
        text
        @click="goToLogin()"
      >
        LOG IN
      </v-btn>
      <v-btn
        text
        @click="logout()"
      >
        LOG OUT
      </v-btn>
    </v-app-bar>

    <v-main class="fill-height">
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    accId: 0,
  }),

  methods : {
    goToLogin() {
      this.$router.push({path: '/login'}).catch(() => {})
    } ,

    goToManage() {
      this.$router.push( {path: '/manageTable'} ).catch(() => {})
    },

    goToHome(){
      this.$router.push( {path: '/'} ).catch(() => {})
    },
    goToContent(){
      if(localStorage.getItem("loginStatus") == 'true'){
        this.accId = localStorage.getItem("accId")
        console.log(this.accId)
        this.$router.push( {path: '/content/' + this.accId }).catch(() => {})
      }
      else{
        this.$router.push({path: '/login'}).catch(() => {})
      }
    },
    logout(){
      localStorage.setItem("username", '');
      localStorage.setItem("loginStatus" , false);
      localStorage.setItem("accId", 0);

      this.$router.push( {path: '/'} ).catch(() => {})
    }
  }
}
</script>
<style scoped>
.fill-height{
    background-image: url("@/assets/starry_sky.jpg");
    background-repeat: no-repeat;
    background: cover;
    background-size: cover;
  }
</style>
