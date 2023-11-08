<template>
  <v-card
    class="mx-auto"
    outlined
    height="100%"
    @click="show=true"
    variant="flat"
  >
    <v-img
      :src="value.img"
      aspect-ratio="1"
    ></v-img>

    <v-card-title color="orange-lighten-2">
      {{value.name}}
    </v-card-title>

    <v-row align-content="center" justify="center" class="pa-2" style="height:100px; padding: 15px;">
      <div class="pa-4">{{ value.description }}</div>
    </v-row>

    <v-card-actions>

      <v-dialog 
      v-model="show"
      fullscreen
      :scrim="false"
      class="black"
      transition="dialog-bottom-transition">

        <template v-slot:default="{ isActive }">
          <v-card :title="value.name" color="black">
            <v-card-text v-html="value.frame" height="100%">
              
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn
                icon="mdi-close"
                text="Close"
                @click="isActive.value = false"
              ></v-btn>
            </v-card-actions>
          </v-card>
        </template>
      </v-dialog>
    </v-card-actions>


  </v-card>
</template>

<script>
export default {
  props: ['value'],
  data: () => ({
    show: false,
  }),
}
</script>