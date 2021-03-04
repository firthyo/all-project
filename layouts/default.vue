<template>
  <v-app dark>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-icon @click="homePage">mdi-chevron-left</v-icon>

      <v-toolbar-title v-text="title"/>
      <v-spacer/>
      <!--      <v-text-field-->
      <!--        v-model="search"-->
      <!--        append-icon="mdi-magnify"-->
      <!--        label="Search"-->
      <!--        single-line-->
      <!--        hide-details-->
      <!--      ></v-text-field>-->
    </v-app-bar>
    <v-main>

      <nuxt/>

    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'All Project'
    }
  },
  methods: {
    homePage() {
      this.$router.push({path: `/project/`})
    }
  }
}
</script>
