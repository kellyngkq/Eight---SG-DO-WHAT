<script>
import firebaseApp from "../firebase.js";
import { getStorage, ref, getDownloadURL } from "firebase/storage";
import { collection, getDocs, getFirestore } from "firebase/firestore";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

const db = getFirestore(firebaseApp);
const storage = getStorage();

export default {
  name: "Places",
  props: ["rain"],
  data() {
    return {
      places: [],
      selected: [],
    };
  },
  methods: {
    async display() {
      this.places = [];
      const placesRef = collection(db, "Places");
      const docSnap = await getDocs(placesRef);

      docSnap.forEach((doc) => {
        this.places.push([doc.id, doc.data()]);
      });
    },

    async handleClose() {
      this.showModal = false;
    },

    async openModal(chose) {
      this.showModal = true;
      this.chosen = chose;
      await this.$refs.moreinfo.show({
        pic: "src/assets/" + chose[0] + ".jpg",
        message: chose[0],
        time: chose[1].time,
        link: chose[1].url,
      });
      console.log("Current rain status: " + this.rain);
    },
  },
  async mounted() {
    this.display();
  },
};
</script>
<template>
  <!--If there is no rain, show everything-->
  <div v-if="rain == false">
    <input type="checkbox" value="All" v-model="selected" />All <br />
    <input type="checkbox" value="Indoor" v-model="selected" />Indoor <br />
    <input type="checkbox" value="Outdoor" v-model="selected" />Outdoor <br />
    <input type="checkbox" value="Nature" v-model="selected" />Nature<br />
    <input type="checkbox" value="Heritage" v-model="selected" />Heritage<br />
    <input type="checkbox" value="Food" v-model="selected" />Food <br /><br />
    <span>Here are the recommended places, have fun!</span>

    <!--If its not raining, and none selected, then display all-->
    <MoreInfo ref="moreinfo"></MoreInfo>
    <div v-if="selected.length == 0">
      <div class="place" v-for="place in places" :key="places[0]">
        <div class="card" @click="openModal(place)">
          <div class="container">
            <div class="placepic">
              <img :src="'src/assets/' + place[0] + '.jpg'" />
            </div>
            <h1>{{ place[0] }}</h1>
            <br />
            <div v-if="place[1].price == 0">
              <h3>Price: Free</h3>
            </div>
            <div v-else>
              <h3>Price: ${{ place[1].price }}</h3>
            </div>
            <br />
            <h3>Area: {{ place[1].area }}</h3>
          </div>
        </div>
      </div>
    </div>

    <!--This else means that there is something in selected-->
    <div v-else>
      <!--These are all combis that are impossible, and hence there will be no listings shown-->
      <div v-if="selected.includes('Indoor') && selected.includes('Outdoor')">
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div
        v-else-if="selected.includes('Indoor') && selected.includes('Nature')"
      >
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div
        v-else-if="selected.includes('Nature') && selected.includes('Heritage')"
      >
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div v-else-if="selected.includes('Nature') && selected.includes('Food')">
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div
        v-else-if="selected.includes('Heritage') && selected.includes('Food')"
      >
        <h3>There are no listings that match your selections.</h3>
      </div>

      <!--These are all combis that are possible, and hence will be shown-->
      <div v-else-if="selected.includes('Indoor') && selected.includes('Food')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'indoor' && place[1].category == 'food'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        v-else-if="selected.includes('Indoor') && selected.includes('Heritage')"
      >
        <div class="place" v-for="place in places" :key="places[0]">
          <div
            v-if="place[1].type == 'indoor' && place[1].category == 'heritage'"
          >
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        v-else-if="selected.includes('Outdoor') && selected.includes('Food')"
      >
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'outdoor' && place[1].category == 'food'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        v-else-if="selected.includes('Outdoor') && selected.includes('Nature')"
      >
        <div class="place" v-for="place in places" :key="places[0]">
          <div
            v-if="place[1].type == 'outdoor' && place[1].category == 'nature'"
          >
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        v-else-if="
          selected.includes('Outdoor') && selected.includes('Heritage')
        "
      >
        <div class="place" v-for="place in places" :key="places[0]">
          <div
            v-if="place[1].type == 'outdoor' && place[1].category == 'heritage'"
          >
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        v-else-if="selected.includes('Outdoor') && selected.includes('Food')"
      >
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'outdoor' && place[1].category == 'food'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!--These are all 1 selection only-->

      <div v-else-if="selected.length == 1 && selected.includes('All')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div class="card">
            <div class="container">
              <h1>{{ place[0] }}</h1>
              <br />
              <div v-if="place[1].price == 0">
                <h3>Price: Free</h3>
              </div>
              <div v-else>
                <h3>Price: ${{ place[1].price }}</h3>
              </div>
              <br />
              <h3>Area: {{ place[1].area }}</h3>
            </div>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Indoor')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'indoor'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Outdoor')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'outdoor'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Nature')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].category == 'nature'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Heritage')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].category == 'heritage'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Food')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].category == 'food'">
            <div class="card">
              <div class="container">
                <h1>{{ place[0] }}</h1>
                <br />
                <div v-if="place[1].price == 0">
                  <h3>Price: Free</h3>
                </div>
                <div v-else>
                  <h3>Price: ${{ place[1].price }}</h3>
                </div>
                <br />
                <h3>Area: {{ place[1].area }}</h3>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!--Else, if its raining, then show only indoor places-->
  <div v-else-if="rain == true">
    <div class="place" v-for="place in places" :key="places[0]">
      <!--filter for those places that are indoor-->
      <div v-if="place[1].type == 'indoor'">
        <div class="card">
          <div class="container">
            <h1>{{ place[0] }}</h1>
            <br />
            <div v-if="place[1].price == 0">
              <h3>Price: Free</h3>
            </div>
            <div v-else>
              <h3>Price: ${{ place[1].price }}</h3>
            </div>
            <br />
            <h3>Area: {{ place[1].area }}</h3>
          </div>
        </div>
      </div>
    </div>
    <!--checkbox also auto checks "indoor and whatever category the place is under"-->
    <div>
      <input type="checkbox" value="All" v-model="selected" />Explore All
      <input type="checkbox" value="Indoor" v-model="selected" checked />Indoor
      <input type="checkbox" value="Outdoor" v-model="selected" />Outdoor
      <input type="checkbox" value="Lifestyle" v-model="selected" />Lifestyle
      <input
        type="checkbox"
        value="Nature/Outdoor"
        v-model="selected"
      />Nature/Outdoor
      <input
        type="checkbox"
        value="Entertainment"
        v-model="selected"
      />Entertainment <br /><br />
      <span>Here are the recommended places, have fun!</span>
    </div>
  </div>
</template>

<style>
.card {
  /* Add shadows to create the "card" effect */
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  text-align: left;
  width: 52vh;
  margin: 10px;
  background-color: palegoldenrod;
  height: 40vh;
  padding: 5px 15px;
}

.place {
  grid-template-columns: auto auto auto;
  margin-top: 3vh;
  float: left;
  margin-left: 7vh;
}

.placepic img {
  display: table-row;
  height: 300px;
  width: 80vh;
  object-fit: cover;
  padding-top: 5vh;
  padding-left: 5vh;
}
</style>
