<script>
import axios from "axios";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import Places from "@/components/Places.vue";

export default {
  name: "weather",
  data() {
    return {
      currDate: new Date(),
      rain: false,
      temp: 0,
      url: "",
    };
  },
  components: {
    Places
  },

  async mounted() {
    const rainResponse = await axios.get(
      "https://api.data.gov.sg/v1/environment/rainfall"
    );
    let rainfall = rainResponse.data.items[0]["readings"];
    this.currDate = new Date(rainResponse.data.items[0]["timestamp"]);
    for (let i = 0; i < rainfall.length; i++) {
      if (rainfall[i]["value"] >= 1) {
        this.rain = true;
        break;
      }
    }
    const tempResponse = await axios.get(
      "https://api.data.gov.sg/v1/environment/air-temperature"
    );
    let temperature = tempResponse.data.items[0]["readings"];
    console.log(new Date(tempResponse.data.items[0]["timestamp"]));
    for (let i = 0; i < temperature.length; i++) {
      this.temp += temperature[i]["value"];
    }
    this.temp /= temperature.length;
  },


};
</script>

<template>
  <div class="Greetings">
    <div class="weather">
      <h1><strong>Today's Weather in Singapore</strong></h1>
      <h2>Rainfall today {{ rain }}</h2>
      <div class="temp">
        <font-awesome-icon icon="fa-solid fa-temperature-half" />
        <h2>{{ temp.toPrecision(3) }}</h2>
      </div>
      <h6>
        Information accurate as of {{ currDate.toLocaleDateString() }},
        {{ currDate.toTimeString() }}
      </h6>
    </div>
    <div v-if="!rain" class="perfect"></div>
    <div v-else class="rain"></div>
  </div>
  <div id="myimg"></div>
  <Places :rain = "rain"></Places>

</template>

<style scoped>
.Greetings {
  position: relative;
  text-align: center;
  color: white;
  height: fit-content;
}

img {
  z-index: 000;
  width: 100%;
  height: 80vh;
  object-fit: cover;
  -webkit-filter: blur(1px); /* Safari 6.0 - 9.0 */
  filter: blur(1px) sepia(50%);
}

.perfect {
  min-height: 1000px;
  height: 100%;
  filter: blur(3px);
  background-image: url("../assets/perfect.JPG");
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.rain {
  min-height: 1000px;
  height: 100%;
  filter: blur(3px);
  background-image: url("../assets/rainy.jpg");
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.temp {
  font-size: 30px;
}

h2 {
  display: inline-block;
  margin-left: 5px;
  font-size: 40px;
}

h1 {
  font-size: 60px;
}
.weather {
  z-index: 4;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-shadow: 200px;
  background-color: rgba(0, 0, 0, 0.35);
  padding-left: 20px;
  padding-right: 20px;
  border-radius: 15px;
}
</style>
