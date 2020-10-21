<template>
  <v-app>
    <v-container fluid>
      <v-row no-gutters>
        <v-col cols="7" class="main-part d-none d-md-none d-lg-flex">
          <div class="d-flex">
            <v-img src="@/assets/logo.png" contain></v-img>
            <p>Welcome To Cloud</p>
          </div>
        </v-col>
        <v-col
          cols="12"
          lg="5"
          class="login-part d-flex align-center justify-center"
        >
          <v-row no-gutters>
            <v-col
              cols="12"
              class="login-part d-flex align-center justify-center flex-column"
            >
              <div class="login-wrapper">
                <v-tabs grow>
                  <v-tabs-slider></v-tabs-slider>
                  <v-tab :href="`#tab-login`">
                    LOGIN
                  </v-tab>
                  <v-tab :href="`#tab-newUser`">
                    New User
                  </v-tab>

                  <v-tab-item :value="'tab-login'">
                    <v-form>
                      <v-container>
                        <v-row class="flex-column">
                          <v-col>
                            <p
                              class="login-slogan display-2 text-center font-weight-medium my-10"
                            >
                              Login
                            </p>
                            <v-btn
                              height="45"
                              block
                              color="white"
                              elevation="0"
                              class="google text-capitalize"
                            >
                              <v-img
                                src="@/assets/google.svg"
                                max-width="30"
                                class="mr-4"
                              ></v-img>
                              Sign in with Google</v-btn
                            >
                          </v-col>
                          <v-col cols="12" class="d-flex align-center my-8">
                            <v-divider></v-divider>
                            <span class="px-5"> or </span>
                            <v-divider></v-divider>
                          </v-col>
                          <v-form>
                            <v-col>
                              <v-text-field
                                v-model="login1.email"
                                :rules="emailRules"
                                value="pupan.po@inet.co.th"
                                label="Email Address"
                                required
                              ></v-text-field>
                              <v-text-field
                                v-model="login1.password"
                                :rules="[rules.req]"
                                type="password"
                                label="Password"
                                hint="At least 6 characters"
                                required
                              ></v-text-field>
                            </v-col>
                            <v-col class="d-flex justify-space-between">
                              <v-btn
                                class="text-capitalize"
                                large
                                :disabled="
                                  login1.password.length === 0 ||
                                    login1.email.length === 0
                                "
                                color="primary"
                                @click="login"
                              >
                                Login</v-btn
                              >
                              <v-btn
                                large
                                text
                                class="text-capitalize primary--text"
                                >Forget Password</v-btn
                              >
                            </v-col>
                          </v-form>
                        </v-row>
                      </v-container>
                    </v-form>
                  </v-tab-item>

                  <v-tab-item :value="'tab-newUser'">
                    <v-form>
                      <v-container>
                        <v-row class="flex-column">
                          <v-col>
                            <p
                              class="login-slogan display-2 text-center font-weight-medium mt-10"
                            >
                              Welcome!
                            </p>
                            <p
                              class="login-slogan display-1 text-center font-weight-medium mb-10"
                            >
                              Create your account
                            </p>
                          </v-col>

                          <v-form ref="form" v-model="valid" lazy-validation>
                            <v-col>
                              <v-text-field
                                append-icon=" mdi-account"
                                name="email"
                                label="email"
                                type="email"
                                v-model="register.userEmail"
                                :error="error"
                                :rules="emailRules"
                              />
                              <v-text-field
                                :type="hidePassword ? 'password' : 'text'"
                                :append-icon="
                                  hidePassword
                                    ? ' mdi-account-outline'
                                    : ' mdi-account-off'
                                "
                                name="password"
                                label="Password"
                                id="password"
                                :rules="[rules.req]"
                                v-model="register.password"
                                :error="error"
                                @click:append="hidePassword = !hidePassword"
                              />

                              <v-text-field
                                append-icon=" mdi-account-circle"
                                name="Fullname"
                                label="Fullname"
                                type="name"
                                v-model="register.Fullname"
                                :error="error"
                                :rules="[rules.req]"
                              />
                            </v-col>
                            <v-col class="d-flex justify-space-between">
                              <v-btn
                                block
                                color="primary"
                                @click="validate"
                                :loading="loading"
                              >
                                Create your account</v-btn
                              >
                            </v-col>
                          </v-form>

                          <v-col cols="12" class="d-flex align-center my-4">
                            <v-divider></v-divider>
                            <br />
                            <v-divider></v-divider>
                          </v-col>
                        </v-row>
                      </v-container>
                    </v-form>
                  </v-tab-item>
                </v-tabs>
              </div>
            </v-col>
            <v-col cols="12" class="d-flex justify-center">
              <v-footer>
                <div class="primary--text">
                  © 2020 Pupan, Inet. All rights reserved.
                </div>
              </v-footer>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import swal from "sweetalert";
import axios from "axios";
export default {
  name: "Login",
  data() {
    return {
      valid: true,
      loading: false,
      // userEmail: "aaaa",
      // password: "1234",
      hidePassword: true,
      error: false,
      showResult: false,
      result: "",

      rules: {
        req: (value) => !!value || "จำเป็นต้องระบุข้อมูลให้ครบถ้วน.",
      },
      emailRules: [
        (v) => !!v || "E-mail is required",
        (v) =>
          /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) ||
          "E-mail must be valid",
      ],
      login1: {
        email: "pupan.po@inet.com",
        password: "123456",
      },
      register: {
        userEmail: "",
        rpassword: "",
        Fullname: "",
      },
    };
  },

  methods: {
   
    login() {
      //  ใช้ interceptors เพื่อคอยดักข้อมูลการเชื่อมต่อเซิฟเวอร์ถ้าไม่มีการเชื่อมต่อให้แสดงผล
      axios.interceptors.response.use(   
        (response) => {
          return response;
        },
        (error) => {
          if (!error.response) {
            swal(
              "Error",
              "ไม่สามารถติดต่อเซิฟเวอร์ได้",
              "error"
            );
            console.log("Please check your internet connection.");
          }

          return Promise.reject(error);
        }
      );

      axios
        .post(
          "http://127.0.0.1:3000/data",
          // "http://172.16.112.77:8002/createNew", this.register //Jo
          // "http://172.16.113.73:3000/api/registor/register",
          this.login1
        )
        .then((res) => {
          console.log(res.data);

          console.log(res.data.ok);
          // console.log("sxxad", this.xxx);
          if (res.data.ok === "success") {
            window.localStorage.setItem("authenticated", true);
            this.$router.push("/dashboard");
          } else {
            swal(
              "Error",
              "ไม่พบข้อมูลการลงทะเบียนโปรดตรวจสอบข้อมูลการใช้งานใหม่",
              "error"
            );
            console.log("Register fail");
          }
        });
    },
    validate() {
      if (
        this.$refs.form.validate() === false ||
        this.register.password === "" ||
        this.register.Fullname === ""
      ) {
        swal({
                title: "error",
                text: "ลงทะเบียนไม่สำเร็จกรุณากรอกข้อมูลให้ครบถ้วน",
                icon: "error",
                button: "ตกลง"
              });
              
      }else{
      
      
        // swal("Good job!", "You clicked the button!", "success");
        // console.log(typeof(encryptedText));
        axios
          .post(
            // "http://172.16.112.77:8002/createNew", this.register //Jo
            // "http://172.16.113.73:3000/api/registor/register",
                "http://127.0.0.1:3000/data", 
            
            this.register //new
          )
          .then((res) => {
            console.log(res.data);
            this.xxx = res.data;
            console.log(res.data.ok);
            // console.log("sxxad", this.xxx);
            if (res.data.ok === "success") {
              swal({
                title: "ลงทะเบียนสำเร็จ",
                // text: "ลงทะเบียนสำเร็จ",
                icon: "success",
                button: "ตกลง"
              });
              this.register = ""
            } else {
              console.log("Register fail");
            }
          });
      }

        console.log("pass");
      
    },
  },
  created() {
    if (window.localStorage.getItem("authenticated") === "true") {
      this.$router.push("/login");
    }
  },
};
</script>

<style src="./Login.scss" lang="scss" />
