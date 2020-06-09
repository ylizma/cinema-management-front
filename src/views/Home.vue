<template>
  <div class="flex mt-5">
    <div class="w-1/4 ">
      <cities v-on:send="getCity" />
    </div>
    <div class="w-3/4">
      <div class="cinema-list">
        <ul class="flex">
          <li
            class="flex-1 mr-2"
            v-for="(cinema, index) in cinemas"
            :key="index"
          >
            <a
              class="text-center block border border-gray rounded py-2 px-4 bg-gray-500 hover:bg-gray-700 text-white"
              href="#"
              @click="getRooms(cinema._links.rooms.href, cinema.name)"
              :class="(cinema.name==choosedCinema)?'bg-gray-700':''"
              >{{ cinema.name }}
              </a>
          </li>
        </ul>
      </div>
      <div class="rooms-list grid grid-cols-3 py-3" v-show="rooms != ''">
          <projection v-for="(room, index) in rooms" :key="index" :room="room" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Cities from "@/components/Cities";
import Projection from "@/components/Projection";
export default {
  name: "Home",
  components: {
    Cities,
    Projection,
  },
  data() {
    return {
      cinemas: [],
      choosedCinema: "",
      rooms: [],
    };
  },
  methods: {
    getCity(link) {
      axios
        .get(link)
        .then((res) => {
          this.cinemas = res.data._embedded.cinemas;
          console.log(res.data._embedded.cinemas);
        })
        .catch((err) => console.error(err));
    },
    getRooms(link, name) {
      this.choosedCinema = name;
      axios
        .get(link)
        .then((res) => {
          this.rooms = res.data._embedded.rooms;
        })
        .catch((err) => console.error(err));
    },
  },
  created() {},
};
</script>
