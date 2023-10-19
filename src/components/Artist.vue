<template>
  <v-container>
    <v-fade-transition mode="out-in">
      <v-row>
        <v-col>
          <v-card>
            <v-responsive :aspect-ratio="1/0.5">
            <v-carousel>
              <v-carousel-item
                v-for="image in artists.images"
                :key="image.id"
                :src="image.url"
                reverse-transition="fade-transition"
                transition="fade-transition"
                class="grey darken-4"
                contain
                aspect-ratio="1"
              ></v-carousel-item>
            </v-carousel>
            <v-card-title class="text-h4 font-weight-light orange--text mb-2">
              {{artists.penName}} รับวาดภาพประกอบฉาก
            </v-card-title>
            <v-card-text>
              <div class="font-weight-light red--text text-h6 mb-2">Rate Price : {{artists.ratePrice}}</div>

              <div class="font-weight-light gray--text text-h6 mb-2">Pen Name : {{artists.penName}}</div>
              <v-row>
                <v-col cols="3">
              <div v-for="contact in artists.contact" :key="contact.id" class="font-weight-light grey--text text-h7 mb-2">
                Contact : <br />
                LineId : {{contact.line}} <br />
                Facbook : {{contact.facebook}}  <br />
              </div>
                </v-col>
                <v-col cols="3">
              <div v-for="contact in artists.contact" :key="contact.id" class="font-weight-light grey--text text-h7 mb-2">
                <br />
                Twitter : {{contact.twitter}} <br />
                Tel : {{contact.tel}}  <br />
              </div>
                </v-col>
              </v-row>
            </v-card-text>
            <v-divider class="mx-4"></v-divider>

            <v-card-title>Detail</v-card-title>
            <v-card-text>
              <v-row align="center" class="mx-0">
                <div class="my-4  font-weight-light text-h6 mb-2">
                  {{artists.detail}}
                  
                  / PSD/ TIFF ระบุได้เลยค่ะว่าต้องการเป็น RGB/ CMYK
                  ขนาดของภาพไม่เกิน 9,000 * 9,000 pixels หรือ 30 * 30 นิ้ว
                  ความละเอียดสูงสุด 300 dpi <br /><br />

                  สำหรับ mood & tone หากลูกค้ามีรูปแบบงานที่สนใจอยู่
                  สามารถส่งให้ดูก่อนได้ค่ะ
                  เนื่องด้วยเนื้องานมีความยากง่ายและลายละเอียดต่างกัน
                  ราคาจึงมีความแตกต่างกันนะคะ
                </div>
                <div class="my-4 font-weight-light text-h6 mb-2">
                  ขั้นตอนการทำงาน <br />
                  1. บรีฟลักษณะงานที่ต้องการและวัตถุประสงค์ที่จะนำไปใช้
                  หากมีรูปแบบหรือตัวอย่างที่สนใจเป็นพิเศษจะช่วยในการออกแบบให้ตรงใจผู้ว่าจ้างได้มากยิ่งขึ้น <br />
                  2. ประเมินราคา พร้อมแจ้งระยะเวลาของการทำงาน <br />
                </div>
              </v-row>
            </v-card-text>
            </v-responsive>
          </v-card>
        </v-col>
      </v-row>
    </v-fade-transition>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    // items: [
    //   {
    //     src: "https://cdn.pixabay.com/photo/2012/03/01/00/21/bridge-19513_1280.jpg",
    //   },
    //   {
    //     src: "https://cdn.pixabay.com/photo/2022/11/15/12/23/winter-7593872_1280.jpg",
    //   },
    //   {
    //     src: "https://cdn.pixabay.com/photo/2021/10/27/13/15/digital-graphicscat-6747298_1280.jpg",
    //   },
    //   {
    //     src: "https://cdn.pixabay.com/photo/2023/08/09/08/17/mouse-8178945_1280.jpg",
    //   },
    // ],
    artists: [],
    artistId: 1,

  }),

  created () {
      this.artistId = this.$route.params.artistId;
      console.log(this.artistId)
      this.getArtist()
    },

  methods: {
    async getArtist(){
        try {
            var data = await this.axios.get('http://localhost:9000/artist/'+ this.artistId)
            if(data.status == 200){
                console.log('data artist ===> ' ,data)
                this.artists = data.data
            }
        } catch (error) {
            
        }
    }
  }
};
</script>

<style>
 
</style>