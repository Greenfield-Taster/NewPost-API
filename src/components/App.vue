<template>
  <div class="wrapper">
    <div>
      <h2>Область</h2>
      <select @change="selectCity" v-model="region" class="input">
        <option value="" disabled selected>Область</option>
        <option v-for="region in regions" :key="region.Ref" :value="region.Ref">
          {{ region.Description }}
        </option>
      </select>
    </div>
    <div>
      <h2>Місто</h2>
      <select class="input" @change="selectPostOffice" v-model="city">
        <option value="" disabled selected>City</option>
        <option v-for="city in cities" :key="city.CityID">
          {{ city.Description }}
        </option>
      </select>
    </div>
    <div>
      <h2>Відділення пошти</h2>
      <select v-model="postOffice" class="input">
        <option value="" disabled selected>Post office</option>
        <option v-for="office in postOffices" :key="office.Ref">
          {{ office.Description }}
        </option>
      </select>
    </div>
    <div class="text">
      <h2>You selected:</h2>
      <p>{{ city }}</p>
      <p>{{ postOffice }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const region = ref("");
const regions = ref([]);
const city = ref("");
const cities = ref([]);
const postOffice = ref("");
const postOffices = ref([]);

const selectPostOffice = () => {
  axios
    .post("https://api.novaposhta.ua/v2.0/json/", {
      apiKey: "6db6fbcd0e280f815aa6859b6ecdc4ec",
      modelName: "Address",
      calledMethod: "getWarehouses",
      methodProperties: {
        CityName: city.value,
      },
    })
    .then((response) => {
      postOffices.value = response.data.data;
      console.log(response.data.data);
    });
};

const selectCity = () => {
  axios
    .post("https://api.novaposhta.ua/v2.0/json/", {
      apiKey: "6db6fbcd0e280f815aa6859b6ecdc4ec",
      modelName: "Address",
      calledMethod: "getCities",
      methodProperties: {
        AreaRef: region.value,
      },
    })
    .then((response) => {
      cities.value = response.data.data;
      console.log(response.data.data);
    });
};

onMounted(() => {
  axios
    .post("https://api.novaposhta.ua/v2.0/json/ ", {
      apiKey: "2ee90bea20c27f282c7ce9fb87832dbb",
      modelName: "Address",
      calledMethod: "getAreas",
      methodProperties: {},
    })
    .then((response) => {
      regions.value = response.data.data;
      console.log(response.data.data);
    });
});
</script>
