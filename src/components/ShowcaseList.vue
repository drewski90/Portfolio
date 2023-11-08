<template>
  <v-container>
    <h1 class="font-weight-thin ma-4">
      Codepen Projects
    </h1>
    <v-row class="pa-4">
      <v-col 
      cols="12" lg="3" md="4" sm="6"
      v-for="item in value" :key="item.name">
        <ShowcaseCard :value="item" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ShowcaseCard from './ShowcaseCard.vue';
export default {
  components: {
    ShowcaseCard
  },
  props: [
    "value"
  ],
  data() {
    return {

    }
  }
}
</script>