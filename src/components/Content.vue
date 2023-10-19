<template>
  <v-container>
    <br />
    <v-row justify="center" class="text-center">
      <v-col class="mb-4">
        <h2 class="white--text display-2 font-weight-bold">
          Your Commission Page
        </h2>
      </v-col>
    </v-row>

    <!-- ส่วนเพิ่ม artist -->

    <v-text-field
      v-model="newPenName"
      label="Create new pen name ."
      solo
      @keydown.enter="openForm('add', defaltItem)"
    >
      <template v-slot:append>
        <v-fade-transition>
          <v-icon v-if="newPenName" @click="openForm('add', defaltItem)">
            mdi-plus-circle
          </v-icon>
        </v-fade-transition>
      </template>
    </v-text-field>

    <!-- ส่วนกรอกข้อมูลใหม่ -->
    <template>
      <v-row justify="center">
        <v-dialog v-model="dialog" persistent max-width="600px">
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-form ref="dialogForm" v-model="valid" lazy-validation>
                  <v-row dense>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="penName"
                        label="นามปากกา*"
                        filled
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="ratePrice"
                        label="Rate Price"
                        hint="ช่วงราคาที่รับงาน"
                        filled
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-select
                        v-model="exp"
                        :items="[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]"
                        label="ประสบการณ์ (ปี)"
                        filled
                      ></v-select>
                    </v-col>
                    <v-col cols="12" sm="6">
                      <v-text-field
                        v-model="contact"
                        :rules="contactRules"
                        label="Facebook*"
                        filled
                        required
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-text-field
                        v-model="image"
                        :rules="imageRules"
                        label="NEW Image (URL)*"
                        filled
                        required
                        hint="เพิ่ม URL ของภาพใหม่"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                      <v-textarea
                        v-model="detail"
                        filled
                        label="รายละเอียด"
                        auto-grow
                      ></v-textarea>
                    </v-col>
                  </v-row>
                </v-form>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="red" text @click="closeDialog()"> CLOSE </v-btn>
              <v-btn
                color="success"
                text
                @click="save(formTitle)"
                :disabled="!valid"
              >
                <v-icon left> mdi-plus-circle </v-icon>
                SAVE
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </template>

    <!-- ส่วนแสดงข้อมูล -->
    <h2 class="text-center white--text">
      ไปจัดการนามปากกากันเลย! <br />
    </h2>

    <template>
      <v-row>
        <v-col v-for="artist in myArtists" :key="artist.id" cols="6">
          <v-hover v-slot="{ hover }">
            <v-card class="mx-auto" color="grey lighten-4" max-width="500">
              <v-carousel>
                <v-carousel-item
                  v-for="image in artist.images"
                  :key="image.id"
                  :src="image.url"
                  reverse-transition="fade-transition"
                  transition="fade-transition"
                  class="grey darken-4"
                >
                  <v-expand-transition>
                    <div
                      v-if="hover"
                      class="d-flex transition-fast-in-fast-out amber accent-2 v-card--reveal text-h2 white--text"
                      style="height: 100%"
                    >
                      <v-btn
                        color="orange"
                        class="white--text"
                        fab
                        @click="imageDialog(image, artist)"
                      >
                        <v-icon>mdi-pencil</v-icon>
                      </v-btn>
                      <v-btn
                        color="deep-orange darken-3"
                        class="white--text"
                        fab
                        right
                        @click="deleteForm('image', image ,artist)"
                      >
                        <v-icon>mdi-delete</v-icon>
                      </v-btn>
                    </div>
                  </v-expand-transition>
                </v-carousel-item>
              </v-carousel>
              <v-card-text class="pt-6" style="position: relative">
                <v-btn
                  absolute
                  color="orange"
                  class="white--text"
                  fab
                  small
                  right
                  top
                  @click="openForm('edit', artist)"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <!-- <br/> -->
                <v-btn
                  absolute
                  color="deep-orange darken-3"
                  class="white--text"
                  fab
                  small
                  right
                  @click="deleteForm('artist', artist , '')"
                >
                  <v-icon>mdi-delete</v-icon>
                </v-btn>
                <div class="font-weight-light grey--text text-h6 mb-2">
                  {{ artist.ratePrice }}
                </div>
                <h3 class="text-h4 font-weight-light orange--text mb-2">
                  {{ artist.penName }}
                </h3>

                <div
                  class="font-weight-light grey--text text-h7 mb-2"
                  v-for="contact in artist.contact"
                  :key="contact.id"
                >
                  <v-row>
                    <v-col>
                      <div class="font-weight-bold">Contact :</div>
                      LineId : {{ contact.line }} <br />
                      Facbook : {{ contact.facebook }} <br />
                    </v-col>
                    <v-col>
                      <br />
                      Twitter : {{ contact.twitter }} <br />
                      Tel : {{ contact.tel }} <br />
                    </v-col>
                  </v-row>
                </div>

                <v-divider class="mx-4"></v-divider>

                <v-card-title class="font-weight-light text-h6 mb-2"
                  >Detail</v-card-title
                >
                <v-card-text>
                  <div class="font-weight-light text-h6 mb-2">
                    {{ artist.detail }} <br />
                    Made of bamboo by hand
                  </div>
                </v-card-text>
              </v-card-text>
            </v-card>
          </v-hover>
        </v-col>
      </v-row>
    </template>

    <!-- Dialog Delete -->
    <v-dialog v-model="dialogDelete" max-width="500px">
      <v-card class="delete">
        <v-card-title class="text-h5"
          >คุณต้องการลบ {{ deleteTitle }} ใช่หรือไม่?</v-card-title
        >
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red" text @click="closeDelete()"
            >ยกเลิก</v-btn
          >
          <v-btn
            color="success"
            text
            @click="deleteItemConfirm(deleteTitle)"
            >ตกลง</v-btn
          >
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <!-- end dialog delete -->
    <template>
      <v-dialog v-model="dialogImage" max-width="500px">
        <v-card>
          <v-card-title>
            <span class="text-h5"> แก้ไขภาพ </span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row dense>
                <v-col cols="12">
                  <v-text-field
                    v-model="image"
                    label="Image (URL)*"
                    filled
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="red" text @click="closeDialogImage()"> CLOSE </v-btn>
            <v-btn color="success" text @click="saveImage()" :disabled="!valid">
              <v-icon left> mdi-plus-circle </v-icon>
              SAVE
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </template>

    <!-- จบ image dialog -->
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    dialogImage: false,
    newPenName: null,
    dialog: false,
    penName: "",
    ratePrice: "",
    exp: 0,
    contact: "",
    image: "",
    detail: "",
    artistId: 0,
    accId: 0,
    contactId: 1,
    imageId: 1,
    myArtists: [],
    editItem: [],
    defaltItem: [],
    formTitle: "",
    deleteTitle: "",
    dialogDelete: false,
    imageRules: [(v) => !!v || "กรุณากรอก URL"],
    contactRules: [(v) => !!v || "กรุณากรอก facebook"],
  }),
  created() {
    this.accId = this.$route.params.accId;
    console.log("this accId=>", this.accId);
    this.getData();
  },
  methods: {
    deleteForm(actions, item , artOfImg) {
      if (actions === "artist") {
        this.artistId = item.id;
        this.deleteTitle = "นามปากกานี้";
        this.dialogDelete = true;
        console.log("DEL artist: ", this.artistId);
      } else {
        // image DELETE
        this.artistId = artOfImg.id;
        this.imageId = item.id;
        this.deleteTitle = "ภาพนี้";
        this.dialogDelete = true;
        console.log("DEL image: ", this.imageId);
      }
    },

    openForm(Actions, item) {
      if (Actions === "add") {
        this.penName = this.newPenName;
        this.dialog = true;
        this.formTitle = "เพิ่มนามปากกาใหม่";
        console.log(this.newPenName);
      } else {
        // loop array for get data
        for (let i = 0; i <= item.contact.length; i++) {
          if (!item.contact[i].facebook) {
            this.contact = "";
          } else {
            const facebook = item.contact[i].facebook;
            console.log("for loop =", facebook);
            console.log("contactId =", item.contact[i].id);
            this.contactId = item.contact[i].id;
            this.contact = facebook;
            break;
          }
        }
        // edit dialog
        this.formTitle = "แก้ไขนามปากกา";
        this.dialog = true;
        this.penName = item.penName;
        this.ratePrice = item.ratePrice;
        this.exp = item.exp;
        this.artistId = item.id;
        console.log("artId =", item.id);
        // console.log(item.images.id);
        this.detail = item.detail;
      }
    },
    closeDelete() {
      this.artistId = 0;
      this.imageId = 0;
      this.dialogDelete = false;
    },
    closeDialogImage() {
      this.penName = "";
      this.ratePrice = "";
      this.exp = 0;
      this.contact = "";
      this.image = "";
      this.detail = "";
      this.imageId = 0;
      this.artistId = 0;
      this.dialogImage = false;
    },
    closeDialog() {
      this.penName = "";
      this.ratePrice = "";
      this.exp = 0;
      this.contact = "";
      this.image = "";
      this.detail = "";
      this.imageId = 0;
      this.artistId = 0;
      this.dialog = false;
    },
    async save(actions) {
      // add part
      if (actions === "เพิ่มนามปากกาใหม่") {
        // สร้างข้อมูล JSON
        var postData = {
          artistId: 0,
          penName: this.penName,
          ratePrice: this.ratePrice,
          detail: this.detail,
          exp: this.exp,
          images: [
            {
              url: this.image,
            },
          ],
          contact: [
            {
              facebook: this.contact,
            },
          ],
        };
        console.log("post data: ", postData);
        // เพิ่มข้อมูลลง DB
        try {
          var addResponse = await this.axios.post(
            "http://localhost:9000/artist?accId=" + this.accId,
            postData
          );
          this.getData();
          console.log("after post:", addResponse);
        } catch (error) {
          console.log(error);
        }
        this.closeDialog();
      } else {
        // edit part
        // create JSON to PUT data
        var editData = {
          artistId: 0,
          penName: this.penName,
          ratePrice: this.ratePrice,
          detail: this.detail,
          exp: this.exp,
          images: [
            {
              url: this.image,
            },
          ],
          contact: [
            {
              id: this.contactId,
              facebook: this.contact,
            },
          ],
        };

        // API DataBase
        console.log(actions);
        try {
          var editResponse = await this.axios.put(
            "http://localhost:9000/artist/" + this.artistId,
            editData
          );
          this.getData();
          console.log("data after edit =", editResponse);
        } catch (error) {
          console.log(error);
        }
        this.closeDialog();
      }
    },
    async deleteItemConfirm(Actions) {
      if (Actions === "นามปากกานี้") {
        try {
          var deleteResponse = await this.axios.delete(
            "http://localhost:9000/artist/" + this.artistId
          );
          this.getData();
          console.log("DELETE : ", deleteResponse);
        } catch (error) {
          consol.log(error);
        }
        this.closeDelete();
      }else{
        // delete image
        try {
          var deleteResponse = await this.axios.delete(
            "http://localhost:9000/artist/" + this.artistId + "/" + this.imageId
          );
          this.getData();
          console.log("DELETE : ", deleteResponse);
        } catch (error) {
          consol.log(error);
        }
        this.closeDelete();
      }
    },
    async getData() {
      // เรียกข้อมูลนามปากกาใน account
      this.myArtists = []
      try {
        var artist = await this.axios.get(
          "http://localhost:9000/artist/myAcc/" + this.accId
        );
        console.log("data of " + this.accId, artist);
        this.myArtists = artist.data;
        console.log("My artist =", this.myArtists);
      } catch (error) {}
    },
    imageDialog(image, artist) {
      this.dialogImage = true;
      this.imageId = image.id;
      this.image = image.url;
      // artist
      this.penName = artist.penName;
      this.ratePrice = artist.ratePrice;
      this.exp = artist.exp;
      this.artistId = artist.id;
      this.detail = artist.detail;

      for (let i = 0; i <= artist.contact.length; i++) {
        if (!artist.contact[i].facebook) {
          this.contact = "";
        } else {
          const facebook = artist.contact[i].facebook;
          console.log("for loop =", facebook);
          console.log("contactId =", artist.contact[i].id);
          this.contactId = artist.contact[i].id;
          this.contact = facebook;
          break;
        }
      }

      console.log("image :", image);
    },
    async saveImage() {
      // JSON
      var editData = {
        artistId: this.artistId,
        penName: this.penName,
        ratePrice: this.ratePrice,
        detail: this.detail,
        exp: this.exp,
        images: [
          {
            id: this.imageId,
            url: this.image,
          },
        ],
        contact: [
          {
            id: this.contactId,
            facebook: this.contact,
          },
        ],
      };
      // API
      try {
        var editImage = await this.axios.put(
          "http://localhost:9000/artist/" + this.artistId,
          editData
        );
        this.getData();
        console.log("after post:", editImage);
      } catch (error) {
        console.log(error);
      }
      this.closeDialogImage();
      console.log("save image");
    },
  },
};
</script>

<style>
.v-card--reveal {
  align-items: center;
  bottom: 0;
  justify-content: center;
  opacity: 0.7;
  position: absolute;
  width: 100%;
}
.delete {
  text-align: center;
}
</style>