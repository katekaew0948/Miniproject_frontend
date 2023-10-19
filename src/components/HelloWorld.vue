<template>
  <v-container>
  <v-row class="text-center">

      <v-col class="mb-4">
        <br />
        <h1 class="deep-orange--text display-2 font-weight-bold">
          WELCOME TO COMART {{username}}
        </h1>
      </v-col>
  </v-row>
    <v-fade-transition mode="out-in">
      <v-row justify="center">
        <v-col cols="12" sm="6" md="6" v-for="artist in artists" :key="artist.id">
          <v-card max-width="550">
            <v-responsive :aspect-ratio="1.5/1">
            <v-carousel>
              <v-carousel-item
                v-for="image in artist.images"
                :key="image.id"
                :src="image.url"
                reverse-transition="fade-transition"
                transition="fade-transition"
                class="grey darken-4"
                
              ></v-carousel-item>
            </v-carousel>
            <v-card-title class="headline align-center">
              <v-btn text class="text-h5 font-weight-bold orange--text mb-2" block @click="goToContent(artist.id)">{{artist.penName}}  </v-btn>
            </v-card-title>
            <v-card-text>
              <div class="text-h6">
                Rate Price : {{artist.ratePrice}}
                
              </div>
              <div class="text-h6">
                Experience : {{artist.exp}} ปี
              </div>
            </v-card-text>
            </v-responsive>
          </v-card>
        </v-col>
      </v-row>
    </v-fade-transition>
  </v-container>
</template>

<style scoped>
  /* .fill-height{
    background-image: url('../assets/starry_sky.jpg');
    background-repeat: no-repeat;
    background: cover;
    background-size: cover;
  } */
</style>

<script>
export default {
  name: 'HelloWorld',

  props: {
    message: String
  },

  data: () => ({
    artists:[],
    username: '',
    // items: [
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2012/03/01/00/21/bridge-19513_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2022/11/15/12/23/winter-7593872_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2021/10/27/13/15/digital-graphicscat-6747298_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2023/08/09/08/17/mouse-8178945_1280.jpg',
    //       },
    //     ],
    //   items2: [
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2017/01/22/01/23/stag-1998855_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2016/10/25/22/22/roses-1770165_1280.png',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2017/05/13/17/31/fruit-2310212_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2023/10/06/06/09/yellow-flowers-8297511_1280.jpg',
    //       },
    //     ],

    //   items3: [
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2022/09/13/17/02/leaves-7452420_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2021/10/08/13/02/woman-6691311_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2016/06/14/14/09/skeleton-1456627_1280.png',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2019/11/13/12/26/avatar-4623511_1280.png',
    //       },
    //     ],

    //     items4: [
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2021/03/14/11/14/fish-6093991_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2021/01/01/16/06/hand-5879027_1280.jpg',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2016/06/14/14/09/skeleton-1456627_1280.png',
    //       },
    //       {
    //         src: 'https://cdn.pixabay.com/photo/2023/01/27/04/53/muslim-7747745_1280.png',
    //       },
    //     ],
  }),
  created () {
    this.getUsername();
    this.getAllArtist();
  },

  mounted (){
    this.$EventBus.$on('getUsername', this.getUsername);
  },

  methods : {
   getUsername() {
      if (localStorage.getItem('username') != null) {
            this.username = localStorage.getItem('username');
    }
    else {
      this.username = ' '
    }
   },
   async getAllArtist(){
    this.artists = []
      try {
        var data = await this.axios.get("http://localhost:9000/artist");
        this.artists = data.data;
        console.log("artists =>" ,this.artists)
        
      } catch (error) {
        console.log(error);
      }
   },
   goToContent(artistId){
      console.log(artistId);
      this.$router.push({path: '/artist/' + artistId}).catch(() => {})

   },
  },
}
</script>
