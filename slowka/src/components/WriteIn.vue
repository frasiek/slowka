<script>
import axios from "axios";

export default {
  name: "WriteIn",
  mounted() {
    const self = this;
    axios.get('slowka.json').then(response => {
      self.words = response.data;
      self.findNext();
    });
  },
  data: () => ({
    toFind: null,
    words: [],
    alreadyMatched: null,
    correct: null,
    postedAnwser: null
  }),
  props: {
    lang: String,
  },
  methods: {
    randomIntFromInterval: function (min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    otherLang: function () {
      return this.lang === 'pl' ? 'en' : 'pl';
    },
    findNext() {
      let rand = this.randomIntFromInterval(0, this.words.length - 1);
      let tmpWords = [...this.words];
      let word = tmpWords[rand];
      if (word == this.alreadyMatched) {
        return this.findNext();
      }
      this.alreadyMatched = word;
      tmpWords.splice(rand, 1);
      this.toFind = word[this.lang];
      this.correct = word[this.otherLang()];

    },
    post() {
      if (this.postedAnwser === this.correct) {
        this.$parent.addPoint(5);
        this.$parent.showAlert('success', 'Dobrze!');
      } else {
        this.$parent.showAlert('danger', 'Spróbuj jeszcze raz!');
      }
      this.postedAnwser = null;

      this.findNext();
    }

  }
}
</script>

<template>
  <div>
    <div>
      <h1>
        {{ toFind }}
      </h1>
    </div>
    <div class="container text-center mt-5">
      <div class="row">
        <div class="col">
          <input type="text" class="form-control" v-model="postedAnwser" @keyup.enter="post()"/>
        </div>
        <div class="col">
          <span class="btn btn-success" @click="post()">sprawdź</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  font-size: 3em;
  text-align: center;
  margin-top: 10vh;
}
</style>