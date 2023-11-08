<template>
  <v-container>
    <h1 class="font-weight-thin pa-4">
      Skills & Technologies
    </h1>
    <v-text-field
    width="100%" 
    dense
    v-model="textFilter" 
    label="Looking for a specific technology or skill?"/>
    <v-row class="pa-4" no-gutters>
      <v-col 
      cols="12" lg="3" sm="4"
      v-for="item, index in matchedItems"
      :key="'matched-' + index">
        <v-card
          @click="selectedSkill = item"
          color="transparent"
          height="100%"
          variant="flat"
          tile
          class="text-orange lighten-4"
          >
          <div class="pa-2 mb-1">
            {{item.name}}
          </div>
        </v-card>
      </v-col>
      <v-col 
      cols="12" lg="3" sm="4"
      v-for="item, index in unmatchedItems"
      :key="'unmatched-' + index">
        <v-card
          color="transparent"
          height="100%"
          variant="flat"
          tile
          @click="selectedSkill = item"
          >
          <div class="pa-2 mb-1">
            {{item.name}}
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import skills from "../skills"

export default {
  props: [
    "value"
  ],
  data() {
    return {
      selectedSkill: null,
      show:false,
      skills,
      textFilter: '',
    }
  },
  computed: { 
    matchedItems() {
      if (this.textFilter.trim().length > 0) {
        const regex = this.userInputToRegex(this.textFilter)
        return this.skills.filter((obj) => 
          regex.test(obj.name) || 
          regex.test(obj.description) || 
          regex.test(obj.category)
        )
      } else {
        return []
      }
    },
    unmatchedItems() {
      return this.skills.filter((obj) => !this.matchedItems.includes(obj))
    },
  },
  methods: {
    userInputToRegex(userInput) {
      const sanitizedInput = userInput.replace(/[^a-zA-Z0-9\s]/g, '');
      const escapedInput = sanitizedInput.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
      return new RegExp(escapedInput, 'i');
    }
  }
}
</script>