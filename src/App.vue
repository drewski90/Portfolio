<template>
  <v-app id="inspire">

    <v-app-bar color="black">
      <v-app-bar-title class="text-left">Andrew Martinez</v-app-bar-title>
      <v-spacer/>
      <v-btn icon @click="toggleTheme"><v-icon>mdi-lightbulb</v-icon></v-btn>
    </v-app-bar>

    <div class="mt-0">

      <StarHeader />
      <ShowcaseList :value="showcase" />
      <SkillsList />

    </div>
  </v-app>
</template>

<script>

import StarHeader from './components/StarHeader.vue'
import ShowcaseList from './components/ShowcaseList.vue'
import showcase from "./showcase"
import SkillsList from './components/SkillsList.vue'

export default {
  name: 'App',
  components: {
    ShowcaseList,
    StarHeader,
    SkillsList
},
  data() {
    return {
      drawer: false,
      showcase
    }
  },
  mounted() {
    this.theme = 'dark'
  },
  methods: {
    toggleTheme() {
      this.theme = this.theme === "dark"? "light": "dark"
    }
  },
  computed: {
    theme: {
      get() {
        return this.$vuetify.theme.name
      },
      set(v) {
        this.$vuetify.theme.name = v
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
