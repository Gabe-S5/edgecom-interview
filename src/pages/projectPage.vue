<template>
  <div>Project page</div>
  <v-list>
    <v-list-item v-for="item in listItems" :key="item">
      {{ item.name }}, {{ item.number }}</v-list-item
    >
    <v-pagination
      v-model="pagination.currentPage"
      :length="pagination.pageCount"
      @update:model-value="getNewPage()"
    ></v-pagination>
  </v-list>
</template>

<script setup lang="ts">
import axios from "axios";
import { ref } from "vue";
const accessToken = localStorage.getItem("loginToken");
const listItems = ref({});
const pagination = ref({});
axios
  .get("https://api.utrack.solutions/projects?page=1&perPage=10", {
    headers: {
      accept: "application/json",
      Authorization: "Bearer " + accessToken,
    },
  })
  .then(function (response) {
    console.log(response);
    listItems.value = response.data.items;

    pagination.value = response.data.pagination;
  })
  .catch(function (error) {
    console.log(error);
  });

const getNewPage = () => {
  console.log(pagination.value.currentPage);
  axios
    .get(
      "https://api.utrack.solutions/projects?page=" +
        pagination.value.currentPage.toString() +
        "&perPage=10",
      {
        headers: {
          accept: "application/json",
          Authorization: "Bearer " + accessToken,
        },
      }
    )
    .then(function (response) {
      console.log(response);
      listItems.value = response.data.items;
      pagination.value = response.data.pagination;
    })
    .catch(function (error) {
      console.log(error);
    });
};
</script>
