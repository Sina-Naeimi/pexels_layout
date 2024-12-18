<template>
  <v-app>
    <VRow>
      <VCol cols="3">
        <NavBar
          :list-items="listItems"
          @search-value="getDataFromApi"
          @show-image-details="showImageDetails"
        />
      </VCol>
      <VCol cols="9" align="center" class="margin-auto">
        <v-row>
          <v-col>
            <v-card
              height="400"
              class="mx-auto"
              color="surface-variant"
              :image="selectedImage.url"
              :title="selectedImage.photographer"
            >
              <template v-slot:actions>
                <v-btn
                  append-icon="mdi-chevron-right"
                  color="red-lighten-2"
                  variant="outlined"
                  block
                >
                  <a :href="selectedImage.photographer_url" target="_blank">photographer</a>
                </v-btn>
              </template>
            </v-card>
            <!--   <v-card class="mx-auto" :title="selectedImage.photographer">
              <template v-slot:actions>
                <v-btn
                  @click="selectedImage.photographer_url"
                  text="Button"
                ></v-btn>
              </template>
            </v-card>
            <v-img
              :aspect-ratio="1"
              class="bg-white"
              :src="selectedImage.url"
              width="300"
            ></v-img> -->
          </v-col>
        </v-row>

        <v-row class="mt-6">
          <v-col
            cols="6"
            sm="4"
            v-for="(item, index) in listItems"
            :key="index"
          >
            <v-img
              gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.7)"
              :src="item.src.small"
            ></v-img>
          </v-col>
        </v-row>
      </VCol>
    </VRow>
    <v-overlay :model-value="loading" class="align-center justify-center">
      <v-progress-circular
        color="primary"
        size="64"
        indeterminate
      ></v-progress-circular>
    </v-overlay>
  </v-app>
</template>

<script setup>
import { ref } from "vue";

import NavBar from "./components/NavBar.vue";
const loading = ref(false);
const listItems = ref();
const selectedImage = ref({
  url: "",
  photographer: "",
  photographer_url: "",
});
const getDataFromApi = async (searchedValue) => {
  loading.value = true;
  const API_KEY = "5WoXUf7FH4GvzuBhPZRoZTAvsOkfkmQRdOZ6Rf2jmdqTbiroUjOztNdZ";
  let ENDPOINT_URL = `https://api.pexels.com/v1/search?query=${searchedValue}&orientation=landscape&per_page=3`;
  const res = await fetch(ENDPOINT_URL, {
    headers: {
      Authorization: API_KEY,
    },
  });
  const responseJson = await res.json();
  listItems.value = responseJson.photos;
  selectedImage.value.url = listItems.value[0].src.original;
  selectedImage.value.photographer = listItems.value[0].photographer
  selectedImage.value.photographer_url = listItems.value[0].photographer_url
  loading.value = false;
};
const showImageDetails = (details) => {
  console.log(details);
  selectedImage.value.url = details.src.portrait;
  selectedImage.value.photographer = details.photographer;
  selectedImage.value.photographer_url = details.photographer_url;
};
getDataFromApi("nature");
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #6a7682;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
