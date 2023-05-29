<template>
  <popup-modal ref="popup">
    <div class="btns">
      <button class="ok-btn" @click="_ok">X</button>
    </div>
    <img :src="pic" />
    <h1>
      <strong>{{ message }}</strong>
    </h1>
    <h4>Estimated time spent: {{ time }}</h4>
    <a v-if="typeof link !== 'undefined'" :href="link">Find out more!</a>
  </popup-modal>
</template>

<script>
import PopupModal from "./PopupModal.vue";

export default {
  name: "AcknowledgeDialogue",

  components: { PopupModal },

  data: () => ({
    // Parameters that change depending on the type of dialogue
    pic: undefined,
    message: undefined, // Main text content
    time: undefined,
    link: undefined,

    // Private variables
    resolvePromise: undefined,
    rejectPromise: undefined,
  }),

  methods: {
    show(opts = {}) {
      this.pic = opts.pic;
      this.message = opts.message;
      this.time = opts.time;
      this.link = opts.link;
      // Once we set our config, we tell the popup modal to open
      this.$refs.popup.open();
      // Return promise so the caller can get results
      return new Promise((resolve, reject) => {
        this.resolvePromise = resolve;
        this.rejectPromise = reject;
      });
    },

    _ok() {
      this.$refs.popup.close();
      this.resolvePromise(false);
      // Or you can throw an error
      // this.rejectPromise(new Error('User canceled the dialogue'))
    },
  },
};
</script>

<style scoped>
p {
  text-align: left;
}
.btns {
  display: inline;
  flex-direction: row;
  justify-content: space-between;
}

.ok-btn {
  padding: 0.5em 0.5em;
  background-color: white;
  color: black;
  border: none;
  border-radius: 5px;
  font-weight: bold;
  font-size: 12px;
  text-transform: uppercase;
  cursor: pointer;
  float: right;
  font-size: 2vh;
  margin-bottom: 1vh;
}

.ok-btn:hover {
  background-color: rgb(255, 0, 0, 0.6);
}

.ok-btn:active {
  background-color: lightblue;
  box-shadow: 0 2px lightgray;
  transform: translateY(4px);
}

img {
  display: table-row;
  height: 300px;
  width: 80vh;
  object-fit: cover;
}
</style>
