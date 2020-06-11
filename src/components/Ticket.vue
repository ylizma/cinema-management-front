<template>
  <div
    class="p-2  m-2 text-center"
    :class="[
      ticket.taken
        ? 'bg-gray-200 hover:bg-gray-500'
        : 'bg-green-200 hover:bg-green-500',
      reservedClass ? 'bg-blue-300 hover:bg-blue-500' : '',
    ]"
  >
    <a href="#" class="text-center" @click="!ticket.taken ? reserver() : ''">
      {{ ticket.seat.number }}
    </a>
  </div>
</template>

<script>
export default {
  props: ["ticket"],
  data() {
    return {
      reservedClass: false,
    };
  },
  methods: {
    reserver() {
      this.reservedClass = !this.reservedClass;
      this.$emit("reserve", this.ticket);
    },
  },
};
</script>

<style></style>
