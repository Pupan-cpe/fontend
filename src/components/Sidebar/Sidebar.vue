<template>
  <v-navigation-drawer
    app
    clipped
    v-model="DRAWER_STATE"
    :mini-variant="!DRAWER_STATE"
    :width="sidebarWidth"
    :permanent="$vuetify.breakpoint.mdAndUp"
    :temporary="$vuetify.breakpoint.smAndDown"
    :mini-variant-width="sidebarMinWidth"
    :class="{'drawer-mini': !DRAWER_STATE}">
    <v-list>
      <template v-for="(item, i) in items">
        <v-row
          v-if="item.heading"
          :key="item.heading"
          align="center">
          <v-col cols="6">
            <span
                  style="padding-left: 32px"
                  class="text-body-1 subheader"
                  :class="(item.heading && DRAWER_STATE) ? 'show ' : 'hide'">
                  {{ item.heading }}
              </span>
          </v-col>
          <v-col
            cols="6"
            class="text-center">
          </v-col>
        </v-row>
        <v-divider
          v-else-if="item.divider"
          :key="i"
          dark
          class="my-4"
        ></v-divider>
        <v-list-group
          color="primary"
          v-else-if="item.children && DRAWER_STATE"
          :key="item.title"
          v-model="item.model"
          append-icon="">
            <template v-slot:prependIcon>
              <v-icon size="28">mdi-image-filter-none</v-icon>
            </template>
            <template v-slot:activator >
              <v-list-item-content >
                <v-list-item-title
                  class="grey--text">
                    {{ item.title }}
                </v-list-item-title>
              </v-list-item-content>
            </template>
            <v-list-item
              v-for="(child, i) in item.children"
              :key="i"
              :to="child.link"
              link>
                <v-list-item-action v-if="child.icon">
                  <v-icon size="">{{ child.icon }}</v-icon>
                </v-list-item-action>
                <v-list-item-content>
                  <v-list-item-title class="grey--text">
                    {{ child.title }}
                  </v-list-item-title>
                </v-list-item-content>
            </v-list-item>
        </v-list-group>
        <v-list-item
          color="primary"
          v-else
          :key="item.text"
          :to="item.link === '#' ? null : item.link"
          link>
          <v-list-item-action>
            <v-icon
              size="28"
              :color="item.color ? item.color : ''"
            >{{ item.icon }}</v-icon>
          </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title
                class="grey--text"
                link>
                {{ item.title }}
              </v-list-item-title>
            </v-list-item-content>
        </v-list-item>
      </template>
    </v-list>
    </v-navigation-drawer>
</template>

<script>
import {mapActions, mapState} from 'vuex'

  export default {
    props: {
        source: String,
    },
    data(){
      return {
        items: [
          { title: 'Data Table', icon: 'mdi-table', link: '/dashboard' },
          { title: 'LiveVideo', icon: ' mdi-video', link: '/typography' },
          { title: 'Manage User', icon: 'mdi-face', link: '/tables' },
          { title: 'Picture', icon: 'mdi-image', link: '/notifications' },
          //  { title: 'Maps', icon: 'mdi-circle-small', link: '/maps'},
            // { title: 'Profile', icon: '  mdi-account-box', link: '/Profile'},
          {
            // title: 'UI Elements',
            // icon: 'mdi-image-filter-none',
            link: '/icons',
            model: false,
            // children: [
            //   { title: 'Icons', icon: 'mdi-circle-small', link: '/icons'},
            //   { title: 'Charts', icon: 'mdi-circle-small', link: '/charts'},
            //   { title: 'Maps', icon: 'mdi-circle-small', link: '/maps'},
            // ],
          },
         

        ],
        sidebarWidth: 240,
        sidebarMinWidth: 96
      }
    },
    computed: {
      ...mapState(['drawer']),
      DRAWER_STATE: {
        get() {
          return this.drawer
        },
        set(newValue) {
          if (newValue === this.drawer) return;
          this.TOGGLE_DRAWER();
        }
      }
    },
    methods: {
      ...mapActions([ 'TOGGLE_DRAWER' ]),
    }
  }
</script>

<style src="./Sidebar.scss" lang="scss"/>
