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
    <tr v-if="loading">LOADING MORE...</tr>
      <tr v-for="user in users" :key="user.id">
        <td>{{ user.id }}</td>
        <td>{{ user.name }}</td>
        <td>{{ user.username }}</td>
        <td>{{ user.email }}</td>
      </tr>
    </v-card>
  </v-container>
</template>

<script>
import axios from "axios";

import Swal from "sweetalert";

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
      users: [],
      loading: false,
      setIntervalId: null,

      isDragging: false,
      dragCount: 0,
      files: [],
      images: [],
      progress: 0,
      dialog: false,
    };
  },

  created() {
    this.getUsers();
    this.setIntervalId = setInterval(() => {
      this.getUsers();
    }, 1000);
  },
  beforeDestroy() {
    clearInterval(this.setIntervalId);
  },

  methods: {
    async getUsers() {
      // this.loading = true;
      try {
        const users = await axios.get(
          "http://172.16.113.116:3000/api/authen/table-all"
        );
        this.users = users.data;
        this.loading = false;
      } catch (err) {
        console.log(err); // handle errors here...
      }
    },
    // getdb(){
    //    axios.post("http://172.16.113.116:3000/api/authen/table-all/", this.user)
    //       .then(response => {
    //         console.log(response);

    //       })
    // },

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

      this.files.forEach((file) => {
        formData.append("images[]", file, file.name);
      });

      axios
        .post("http://172.16.113.116:3000/api/employ/insert3", this.images, {
          onUploadProgress: (uploadEvent) => {
            this.progress = Math.round(
              (uploadEvent.loaded / uploadEvent.total) * 100
            );
            console.log(this.images);
          },
        })
        .then((res) => {
          console.log(res.data);
          if (res.data.ok == "upload - SECCESS") {
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
