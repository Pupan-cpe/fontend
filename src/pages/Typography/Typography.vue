<template>
    <div id="app">
        <div class="container">
            <my-video :sources="video.sources" :options="video.options"></my-video>
        </div>
    </div>
</template>

<script>
// script
import myVideo from 'vue-video'
export default {
    data () {
        return {
            video: {
                sources: [{
                    src: 'http://192.168.1.124:8000/video_feed?autoplay=1',
                    type: 'video/mp4'
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
    }
}
 
</script>

<style lang="scss" scoped>

</style>