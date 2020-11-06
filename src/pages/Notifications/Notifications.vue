<template>
  <div id="app">
  <v-app id="inspire">
    <v-layout>
      <v-flex xs12 sm6 offset-sm3>
        <v-text style="margin-left">AI Detection</v-text>
        <hr>
        <br>
        <br>
      <img v-bind:src="'data:image/png;base64,'+pics" />
      <hr>
      <v-text style="margin-left">User Detection</v-text>
      <hr>
      <br>
      <br>
      <img v-bind:src="'data:image/png;base64,'+userpics" />
      <!-- <div>{{ pics }}</div> -->
      </v-flex>
    </v-layout>
  </v-app>
</div>
</template>

<script>
import axios from "axios";
export default {
  data () {
    return {
      base64: "",
      pics:"",
      userpics:"",
    }
  },
  mounted() {
    this.getpic();
    this.picuser();
    this.setIntervalId = setInterval(() => {
      this.getpic();
       this.picuser();
    }, 500);
  },

  components: {},
  methods: {
    getBase64() {
      axios
        .get("http://192.168.1.132:3000/api/sensor/last_photo_2")
        .then((res) => {
          this.base64 = res.data.data[0].picture;
          console.log(this.base64);
          
        });
    },

    async getpic() {
      // this.loading = true;
      try {
        const res = await axios.get(
          "http://192.168.1.132:3000/api/sensor/last_photo_2"
        );
        
       this.pics = res.data.data[0].picture;
        // console.log(typeof(this.pics));
        // console.log(this.pics);
      
        // console.log(this.pics);
      } catch (err) {
        console.log(err); // handle errors here...
      }
    },

     async picuser() {
      // this.loading = true;
      try {
        const res = await axios.get(
          "http://192.168.1.132:3000/api/sensor/Qcap"
        );
        
       this.userpics = res.data.data[0].picture;
        // console.log(this.userpics);
        // console.log(this.pics);
      
        // console.log(this.pics);
      } catch (err) {
        console.log(err); // handle errors here...
      }
    },
    
  },
  
};
</script>

<style lang="scss" scoped></style>
