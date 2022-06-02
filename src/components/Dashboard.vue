<template>
  <h1 id="theTime">{{ getDate }}</h1>
  <!-- <button @click="consoleme">Click</button> -->
</template>

<script>
import moment from "moment";
export default {
  name: "Dashboard",
  data() {
    return {
      time: null,
      interval: null,
    };
  },
  methods: {
    consoleme() {
      console.log(moment(this.time).format("h:mm"));
    },
  },
  computed: {
    getDate() {
      return moment(this.time).format("h:mm");
    },
  },
  created() {
    this.interval = setInterval(() => {
      this.time = new Date();
    }, 1000);
  },
};
</script>

<style scoped>
#theTime {
  animation: fadein 2s;
  color: red;
  position: absolute;
  top: 50%;
  right: 50%;
  transform: translate(50%, -100%);
  font-size: 10.5rem;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
