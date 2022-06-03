<template>
  <button @click="getSports" style="position: absolute; z-index: 9; top: 300px" >
    Click for tes
  </button>
<div id="outer-container" :class="{ showing: !loading, noShow: loading }">
  <div  id="scorebox" v-for="game in nbaGames" :key="game">

    <div class="scorebox-inner">
      <img class="teamLogo" :src="game.teams.away.logo" alt="Away Team" />
      <h2 class="scores">{{ game.scores.away.total }}</h2>
    </div>
    <h4>{{ getGameTime(game.date) }}</h4>
    <div class="scorebox-inner">
      <img class="teamLogo" :src="game.teams.home.logo" alt="Home Team" />
      <h2 class="scores">{{ game.scores.home.total }}</h2>
    </div>
  </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "NbaScores",
  data() {
    return {
      nbaGames: null,
      loading: true,
    };
  },
  methods: {
    async getSports() {
      var today = new Date();
      let dd = String(today.getDate()).padStart(2, "0");
      let mm = String(today.getMonth() + 1).padStart(2, "0"); //January is 0!
      let yyyy = today.getFullYear();

      let newDay = dd.split("");
      for (let i = newDay.length - 1; i >= 0; i--) {
        newDay[i]++;
        if (newDay[i] > 9) {
          newDay[i] = 0;
          newDay[i - 1]++;
        } else {
          break;
        }
      }
      newDay.join("");
      let utcDay = newDay.join("");
      let todayDate = yyyy + "-" + mm + "-" + utcDay;
      console.log(todayDate);

      const res = await fetch(
        `https://v1.basketball.api-sports.io/games?date=${todayDate}&league=12&season=2021-2022`,
        {
          method: "GET",
          headers: {
            "x-rapidapi-key": `${process.env.VUE_APP_SPORTS_API}`,
            "x-rapidapi-host": `v1.basketball.api-sports.io`,
          },
        }
      );
      const { response } = await res.json();
      this.nbaGames = response;
      this.loading = false;
      console.log(this.nbaGames);
    },
    getGameTime(date) {
      let formattedTime = moment(date).format("h:mmA");
      return formattedTime;
    },
  },
};
</script>

<style scoped>

.outer-container {
    position: absolute;
    top: 0; 
    left: 0;
    z-index: 9;
}
.noShow {
    opacity: 0;
    transition: opacity 0.5s ease-in-out;
}
.showing {
    opacity: 1;
    transition: opacity 0.5s ease-in-out;
}
.scorebox-inner {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.scores {
  margin: 0px !important;
  color: rgb(58, 57, 57);
}

.teamLogo {
  width: 70px;
  height: 70px;
  margin: 10px;
}

#scorebox {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 9 !important;
  display: flex;
  justify-content: center;
  align-items: center;
}

.v-enter-active {
  transition: opacity .5s;
}
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;

}
</style>
