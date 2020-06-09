<template>
  <div class="">
    <div class="max-w-sm rounded overflow-hidden shadow-md mx-2 my-2">
      <img
        class="w-full"
        src="https://picsum.photos/200"
        alt="Sunset in the mountains"
        v-show="!ischoosed"
      />
      <div class="px-6 py-4">
        <div v-show="!ischoosed">
          <div class="font-bold text-xl mb-2" v-show="projections !== ''">
            {{ room.name }}
          </div>
          <p class="text-md font-bold pb-1">Seances :</p>
          <div
            class="border-2 mb-1 bg-gray-100 p-1 text-justify rounded-md hover:bg-blue-100"
            v-for="(pro, index) in projections"
            :key="index"
          >
            <a href="#" @click="getTickets(pro)">
              Time: {{ pro.seance.startHour }} | Price:
              {{ pro.price.toFixed(2) }} DH
            </a>
          </div>
        </div>
        <div v-show="ischoosed">
          <ticket v-for="(ticket, index) in tickets" :key="index" :ticket="ticket" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Ticket from "@/components/Ticket";
export default {
  props: ["room"],
  components: {
    Ticket,
  },
  data() {
    return {
      projections: [],
      ischoosed: false,
      tickets: [],
      choosedProjection:{}
    };
  },
  methods: {
    getProjecions() {
      var link = this.room._links.projections.href.replace("{?projection}", "");
      axios
        .get(link + "?projection=projection")
        .then((res) => {
          this.projections = res.data._embedded.projections;
          // console.log(this.projections);
        })
        .catch((err) => {
          console.error(err);
        });
    },
    getTickets(projection) {
       var link = projection._links.tickets.href.replace("{?projection}", "");
      this.choosedProjection=projection
      axios
        .get(link+ "?projection=ticket")
        .then((res) => {
          console.log(res.data._embedded.tickets);
          this.tickets = res.data._embedded.tickets
        })
        .catch((err) => console.error(err));
        this.ischoosed = !this.ischoosed
    },
  },
  created() {
    this.getProjecions();
  },
};
</script>

<style></style>
