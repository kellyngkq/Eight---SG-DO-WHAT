<script>
import firebaseApp from "../firebase.js";
import { getStorage, ref, getDownloadURL } from "firebase/storage";
import { collection, getDocs, getFirestore } from "firebase/firestore";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

const db = getFirestore(firebaseApp);
const storage = getStorage();

export default {
  data() {
    return {
      places: [],
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
  },
  async mounted() {
    this.display();
  },
};
</script>
<template>
  <!--<input type="button" value="view Image" id="viewbtn" @click="showimage" />-->
  <div class="place" v-for="place in places" :key="places[0]">
    <button class="card">
      <h1>{{ place[0] }}</h1>
      <br />
      <h3>Price: ${{ place[1].price }}</h3>
      <br />
      <h3>Area: {{ place[1].area }}</h3>
    </button>
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
