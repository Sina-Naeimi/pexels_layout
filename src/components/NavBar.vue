<template>
  <v-card>
    <v-navigation-drawer floating permanent>
      <v-text-field
        v-model="searchedValue"
        variant="outlined"
        density="compact"
        label="Search"
        append-inner-icon="mdi-magnify"
        @click:append-inner="sendSearchInput"
      ></v-text-field>
      <v-list
        density="compact"
        nav
        v-for="(item, index) in props.listItems"
        :key="index"
      >
        <v-list-item
         @click.stop="sendImageDetails(item)"
          prepend-icon="mdi-view-dashboard"
          title="image"
          value="home"
        ></v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-card>
</template>

<script setup>
import { defineEmits, defineProps, ref } from "vue";
const searchedValue = ref();

const props = defineProps(["listItems"]);
const emit = defineEmits(["searchValue", "showImageDetails"]);
const sendSearchInput = () => {
  emit("searchValue", searchedValue);
};
const sendImageDetails = (imageDetails)=>{
    emit("showImageDetails", imageDetails);
}
</script>