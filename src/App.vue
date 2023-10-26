<template>
  <div>
    <h2>Oblast</h2>
    <select @change="selectRegion" v-model="region">
      <option value="" disabled selected>Область</option>
      <option v-for="region in regions" :key="region.Ref" :value="region.ref">
        {{ region.Description }}
      </option>
    </select>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const region = ref("");
const regions = ref([]);

const selectRegion = () => {
  axios
    .post("https://api.novaposhta.ua/v2.0/json/ ", {
      apiKey: "2ee90bea20c27f282c7ce9fb87832dbb",
      modelName: "Address",
      calledMethod: "getAreas",
      methodProperties: {},
    })
    .then((response) => {
      console.log(response);
    });
};
</script>
