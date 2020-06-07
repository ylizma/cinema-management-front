<template>
  <div class="">
    <ul class="ml-3 items-center">
      <li class="h-12 bg-gray-200 rounded-md w-48 text-center p-2 mb-1" v-for="(city, index) in cities" :key="index" :class="(city.name==choosed)?'bg-gray-400':''">
        <a href="#" class="text-lg" @click="chooseCity(city._links.cinemas.href,city.name)">{{city.name}}</a>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "citiesList",
  data() {
    return {
      cities: [],
      count:0,
      choosed:''
    };
  },
  methods: {
    getCities() {
      axios
        .get("http://localhost:8888/cities")
        .then((res) => {
          this.cities = res.data._embedded.cities;
          console.log(this.cities);
        })
        .catch((err) => {
          console.error(err);
        });
    },
    chooseCity(link,name){
      console.log(link);
      this.choosed=name
      this.$emit("send",link)
    }
  },
  created() {
    this.getCities();
  },
};
</script>

<style></style>
