<template v-slot:activator="{ dragging: isDragging }">
  <v-container fluid grid-list-xl>
    <v-layout row wrap>
      <v-btn style=" margin: 11px" color="primary" dark @click="dialog = true">
        เพิ่มพนักงาน
      </v-btn>
      <!-- Widgets-->

      <v-row justify="">
        <v-dialog v-model="dialog" persistent max-width="600px">
          <v-card>
            <v-card-title>
              <span class="headline">User Profile</span>
            </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="12">
                    <v-text-field
                      label="ชื่อ*"
                      v-model="employ.name"
                      required
                      name="name"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="12">
                    <v-text-field
                      name="lastname"
                      v-model="employ.lastname"
                      label="นามสกุล"
                      hint="example of helper text only on focus"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      hint="example of persistent helper text"
                      persistent-hint
                      v-model="employ.Email"
                      label="Email*"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-text-field
                      name="password"
                      v-model="employ.password"
                      label="Password*"
                      type="password"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="12" md="12">
                    <v-text-field
                      type="number"
                      name="id"
                      label="รหัสพนักงาน*"
                      v-model="employ.id"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="12">
                    <v-select
                      :items="['21-29', '30-54', '54+']"
                      label="อายุ*"
                      v-model="employ.age"
                      name="age"
                      required
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="12">
                    <v-autocomplete
                      :items="['IOT Engineer', 'BA']"
                      name="position"
                      label="ตำแหน่ง"
                      v-model="employ.position"
                      multiple
                    ></v-autocomplete>
                    <v-col cols="12" sm="6">
                      <!-- <input type="file" id="file" @change="onInputChange" multiple /> -->
                      <label
                        >File
                        <v-icon right dark>mdi-cloud-upload</v-icon>
                        <input
                          @change="onInputChange"
                          multiple
                          type="file"
                          id="file"
                          ref="file"
                          color="primary"
                        />
                      </label>
                    </v-col>
                  </v-col>
                </v-row>
              </v-container>
              <small>*indicates required field</small>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialog = false">
                Close
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                v-on:click="upload(), (dialog = false)"
                >save</v-btn
              >
              <!-- <v-btn color="blue darken-1"  text  @click="dialog = false" ,   >
                Save
              </v-btn> -->
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </v-layout>
    <v-card>
      <v-card-title>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="desserts"
        :search="search"
      ></v-data-table>
    </v-card>
  </v-container>
</template>

<script>
import axios from "axios";

import Swal from 'sweetalert';

export default {
  data() {
    return {
      search: "",
      desserts: [
        {
          name: "Frozen Yogurt",
          calories: 200,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: "1%",
        },
        {
          name: "Ice cream sandwich",
          calories: 200,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: "1%",
        },
        {
          name: "Eclair",
          calories: 300,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: "7%",
        },
        {
          name: "Cupcake",
          calories: 300,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: "8%",
        },
        {
          name: "Gingerbread",
          calories: 400,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: "16%",
        },
        {
          name: "Jelly bean",
          calories: 400,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: "0%",
        },
        {
          name: "Lollipop",
          calories: 400,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: "2%",
        },
        {
          name: "Honeycomb",
          calories: 400,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: "45%",
        },
        {
          name: "Donut",
          calories: 500,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: "22%",
        },
        {
          name: "KitKat",
          calories: 500,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: "6%",
        },
      ],
      headers: [
        {
          text: "Dessert (100g serving)",
          align: "start",
          filterable: false,
          value: "name",
        },
        { text: "Calories", value: "calories" },
        { text: "Fat (g)", value: "fat" },
        { text: "Carbs (g)", value: "carbs" },
        { text: "Protein (g)", value: "protein" },
        { text: "Iron (%)", value: "iron" },
      ],

      employ: {
        name: "",
        lastname: "",
        id: "",
        email: "",
        password: "",
        age: "",
        position: "",
      },

      isDragging: false,
      dragCount: 0,
      files: [],
      images: [],
      progress: 0,
      dialog: false,
    };
  },

  methods: {
    OnDragEnter(e) {
      e.preventDefault();
      this.dragCount++;
      this.isDragging = true;
      return false;
    },

    onInputChange(e) {
      const files = e.target.files;
      Array.from(files).forEach((file) => this.addImage(file));
    },

    addImage(file) {
      console.log("addimage file=", file);
      if (!file.type.match("image.*")) {
        this.$toastr.e(`${file.name} is not an image`);
        return;
      }
      this.files.push(file);
      const img = new Image(),
        reader = new FileReader();
      reader.onload = (e) => this.images.push(e.target.result);
      reader.readAsDataURL(file);
      console.log("addimage this.images=", this.images);
      console.log(img);
    },
    from1() {
      axios
        .post(
          // "http://172.16.112.77:8002/createNew", this.register //Jo
          "http://172.16.113.116:3000/",
          this.employ //new
        )
        .then((res) => {
          console.log(res.data);
          this.xxx = res.data;
          console.log(res.data.ok);
          // console.log("sxxad", this.xxx);
          if (res.data.ok === "SECCESS") {
            this.employ = "";
            this.$router.push("/Login");

            // swal({
            //   title: "ลงทะเบียนสำเร็จ",
            //   // text: "ลงทะเบียนสำเร็จ",
            //   icon: "success",
            //   button: "ตกลง"
            // });
          } else {
            console.log("Register fail");
          }
        });

      console.log("To send images ... ");
    },

    upload() {
      const formData = new FormData();

      this.files.forEach(file => {
        formData.append("images[]", file, file.name);
      });

      axios
        .post("http://172.16.113.116:3000/api/employ/insert3", this.images, {
          onUploadProgress: uploadEvent => {
            this.progress = Math.round(
              (uploadEvent.loaded / uploadEvent.total) * 100
            );
            console.log(this.images);
          }
        })
        .then(res => {
          console.log(res.data);
          if(res.data.ok == "upload - SECCESS"){
          console.log("SUCCESS!!");
          Swal("Good job!", "ลงทะเบียนสำเร็จ", "success");
          }
        })


        .catch(() => {
          this.$toastr.e("Could not upload the files!");
          this.images = [];
          this.files = [];
          this.progress = 0;
        });
      
       
    },
  },
};
</script>

<style>
.table {
  border-radius: 3px;
  background-clip: border-box;
  border: 1px solid rgba(0, 0, 0, 0.125);
  box-shadow: 1px 1px 1px 1px rgba(0, 0, 0, 0.21);
  background-color: transparent;
}
</style>
