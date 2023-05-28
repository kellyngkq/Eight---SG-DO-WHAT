<script>
import axios from "axios";

export default {
  data() {
    return {
      currDate: new Date(),
      rain: false,
      temp: 0,
    };
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
    <h3>Today's Weather in SG</h3>
    <h1>Rainfall today {{ rain }}</h1>
    <h1>Temperature today {{ temp.toPrecision(3) }}</h1>
    <h6>
      Information accurate as of {{ currDate.toLocaleDateString() }},
      {{ currDate.toTimeString() }}
    </h6>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
