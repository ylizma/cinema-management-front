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
          <div
            class="font-semibold font-serif text-sm border-b-2 p-1"
            v-if="choosedProjection"
          >
            Movie: {{ choosedProjection.movie.title }} <br />
            Room: {{ choosedProjection.room.name }} <br />
            Time: {{ choosedProjection.seance.startHour }}
          </div>
          <div class="grid grid-cols-4" v-if="ischoosed && !hideTickets">
            <ticket
              v-for="(ticket, index) in tickets"
              :key="index"
              :ticket="ticket"
              v-on:reserve="reserver"
            />
          </div>
          <button
            v-if="ischoosed && !hideTickets"
            type="button"
            class="bg-red-200 rounded-sm px-2 py-1"
            @click="[(showForm = true), (hideTickets = true)]"
          >
            Payer
          </button>
          <div class="p-2" v-show="hideTickets">
            <form @submit.prevent="saveReservation">
              <i>Name:</i>
              <input
                type="text"
                v-model="clientName"
                class="border-2 inline-block"
                required
              />
              PaymentCode:
              <input
                type="text"
                v-model="paymentId"
                class="border-2 inline-block"
                required
              />
              <button type="submit" class="px-2 py-1 my-2 bg-blue-200">
                pay tickets
              </button>
            </form>
          </div>
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
      choosedProjection: "",
      choosedTickets: [],
      showForm: false,
      hideTickets: false,
      clientName: "",
      paymentId: "",
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
      this.choosedProjection = projection;
      axios
        .get(link + "?projection=ticket")
        .then((res) => {
          console.log(res.data._embedded.tickets);
          this.tickets = res.data._embedded.tickets;
        })
        .catch((err) => console.error(err));
      this.ischoosed = !this.ischoosed;
    },
    reserver(ticket) {
      let p = this.choosedTickets.find((t) => t.id == ticket.id);
      if (p != undefined) {
        this.choosedTickets = this.choosedTickets.filter(
          (t) => t.id != ticket.id
        );
        console.log("deleted");
      } else this.choosedTickets.push(ticket);
      console.log(this.choosedTickets);
    },
    saveReservation() {
      let ticket = {
        clientName: this.clientName,
        paymentId: this.paymentId,
        tickets: this.choosedTickets.map((t) => t.id),
      };
      axios
        .post("http://localhost:8888/payTicket", ticket)
        .then((res) => {
          console.log(res);
        })
        .catch((err) => console.error(err));
        this.ischoosed = !this.ischoosed
        this.hideTickets = !this.hideTickets
        alert("your reservation is successfully taken !!")
    },
  },
  created() {
    this.getProjecions();
  },
  watch: {
    room: function() {
      this.ischoosed = false;
      console.log("changed");
    },
  },
};
</script>

<style></style>
