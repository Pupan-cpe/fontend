<template>
  <div id="app">
    <v-btn color="success" @click="cap" style="margin:10px">Capture</v-btn>
    <div class="container">
      <my-video :sources="video.sources" :options="video.options"></my-video>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
// script
import swal from "sweetalert";
import myVideo from 'vue-video'
export default {
    data () {
        return {
            video: {
                sources: [{
                    src: 'http://192.168.1.124:8000/video_feed?autoplay=1',

                }],
                options: {
                    autoplay: true,
                    volume: 0.6,
                    poster: 'http://192.168.1.124:8000/video_feed?autoplay=1'
                }
            }
        }
    },
    components: {
        myVideo
    },
     methods: {
       cap() {
      
      
        // swal("Good job!", "You clicked the button!", "success");
        // console.log(typeof(encryptedText));
        axios
          .post(
            // "http://172.16.112.77:8002/createNew", this.register //Jo
            // "http://172.16.113.73:3000/api/registor/register",
                "http://192.168.1.132:3000/api/sensor/cap", "cap"
                // "http://192.168.1.124:8000/cap",
          )
          .then((res) => {
            console.log(res.data);
            this.xxx = res.data;
            console.log(this.xxx);
            // console.log("sxxad", this.xxx);
            if (res.data.ok === "success") {
              swal({
                title: "ถ่ายสำเร็จ",
                // text: "ลงทะเบียนสำเร็จ",
                icon: "success",
                button: "ตกลง"
              });
              
            } else {
              console.log("Register fail");
            }
          });
      

        
      
    },
    
}
}
   
</script>

<style lang="scss" scoped></style>
