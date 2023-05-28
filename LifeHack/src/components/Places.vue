<script>
import firebaseApp from "../firebase.js";
import { getStorage, ref, getDownloadURL } from "firebase/storage";
import { collection, getDocs, getFirestore } from "firebase/firestore";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

const db = getFirestore(firebaseApp);
const storage = getStorage();

export default {
  props: {
    rainStatus: Boolean,
  },
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

      console.log(this.rainStatus);
    },
  },
  async mounted() {
    this.display();
  },
};
</script>
<template>
  <!--If there is no rain, show everything-->
  <div v-if="rainStatus == false">
    <input type="checkbox" value="Indoor" v-model="selected" />Indoor <br />
    <input type="checkbox" value="Outdoor" v-model="selected" />Outdoor <br />
    <input type="checkbox" value="Nature" v-model="selected" />Nature<br />
    <input type="checkbox" value="Heritage" v-model="selected" />Heritage<br />
    <input type="checkbox" value="Food" v-model="selected" />Food <br /><br />
    <span>Here are the recommended places, have fun!</span>

    <!--If its not raining, and none selected, then display all-->
    <div v-if="selected.length == 0">
      <div class="place" v-for="place in places" :key="places[0]">
        <button class="card">
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
        </button>
      </div>
    </div>

    <!--This else means that there is something in selected-->
    <div v-else>
      <!--These are all combis that are impossible, and hence there will be no listings shown-->
      <div v-if="selected.includes('Indoor') && selected.includes('Outdoor')">
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div
        v-else-if="selected.includes('Indoor') && selected.includes('Nature')">
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div
        v-else-if="
          selected.includes('Nature') && selected.includes('Heritage')
        ">
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div v-else-if="selected.includes('Nature') && selected.includes('Food')">
        <h3>There are no listings that match your selections.</h3>
      </div>
      <div
        v-else-if="selected.includes('Heritage') && selected.includes('Food')">
        <h3>There are no listings that match your selections.</h3>
      </div>

      <!--These are all combis that are possible, and hence will be shown-->
      <div v-else-if="selected.includes('Indoor') && selected.includes('Food')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'indoor' && place[1].category == 'food'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div
        v-else-if="selected.includes('Outdoor') && selected.includes('Food')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'outdoor' && place[1].category == 'food'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div
        v-else-if="selected.includes('Outdoor') && selected.includes('Nature')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div
            v-if="place[1].type == 'outdoor' && place[1].category == 'nature'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div
        v-else-if="
          selected.includes('Outdoor') && selected.includes('Heritage')
        ">
        <div class="place" v-for="place in places" :key="places[0]">
          <div
            v-if="
              place[1].type == 'outdoor' && place[1].category == 'heritage'
            ">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div
        v-else-if="selected.includes('Outdoor') && selected.includes('Food')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'outdoor' && place[1].category == 'food'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <!--These are all 1 selection only-->
      <div v-else-if="selected.length == 1 && selected.includes('Indoor')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'indoor'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Outdoor')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].type == 'outdoor'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Nature')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].category == 'nature'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Heritage')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].category == 'heritage'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>

      <div v-else-if="selected.length == 1 && selected.includes('Food')">
        <div class="place" v-for="place in places" :key="places[0]">
          <div v-if="place[1].category == 'food'">
            <button class="card">
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
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!--Else, if its raining, then show only indoor places-->
  <div v-else>
    <div class="place" v-for="place in places" :key="places[0]">
      <!--filter for those places that are indoor-->
      <div v-if="place[1].type == 'indoor'">
        <button class="card">
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
        </button>
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
        v-model="selected" />Nature/Outdoor
      <input
        type="checkbox"
        value="Entertainment"
        v-model="selected" />Entertainment <br /><br />
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
  width: 90vh;
  margin: 10px;
  background-color: peachpuff;
  height: 380px;
  border-radius: 20px;
}
</style>
