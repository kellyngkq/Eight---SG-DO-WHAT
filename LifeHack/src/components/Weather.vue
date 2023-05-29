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
    Places,
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
      <div class="temp">
        <font-awesome-icon icon="fa-solid fa-temperature-half" />
        <h2>{{ temp.toPrecision(3) }}°C</h2>
      </div>
      <h2 v-if="rain">
        Don't forget your umbrealla and raincoats, it's time to stay indoors!
      </h2>
      <h2 v-else>The weather is perfect today, go out and enjoy!</h2>

      <h6>
        Information accurate as of {{ currDate.toLocaleDateString() }},
        {{ currDate.toTimeString() }}
      </h6>
    </div>
    <img
      v-if="(temp < 32) & !rain"
      class="perfect"
      src="../assets/perfect.jpg"
    />
  </div>
  <div id="myimg"></div>
  <Places :rain="rain"></Places>
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

/* .weather {
  z-index: 888;
  color: black;
  position: relative;
  text-align: center;
} */

.weather {
  z-index: 888;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-shadow: 200px;
}
</style>
