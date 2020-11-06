
<template>

  <v-footer class="px-0 mt-10" color="transparent" absolute> 
   
    <v-row no-gutters>
      <div>
      
      </div>
      <div>
        © 2020 Pupan,Thanyawat I-net. All rights reserved.
        <v-btn
                v-for="icon in icons"
                :key="icon.icon"
                :href="icon.link"
                class="mr-1"
                :color=config.light.textColor
                icon
        >
          <v-icon size="24px">{{ icon.icon }}</v-icon>
        </v-btn>
      </div>
    </v-row>
  </v-footer>
</template>

<script>
import config from '../../config';

export default {
  name: 'Footer',

  data: () => ({
    config,
    icons: [
     
      {icon: 'mdi-facebook', link: 'https://www.facebook.com/pupan'},
      {icon: 'mdi-github', link: 'https://github.com/pupan-cpe/'},

    ],
   
  }),
}
</script>

<style src="./Footer.scss" lang="scss"/>

