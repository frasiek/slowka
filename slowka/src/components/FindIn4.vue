<script>
import axios from "axios";

export default {
  name: "FindIn4",
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
    anwsers: []
  }),
  props: {
    lang: String,
  },
  methods: {
    randomIntFromInterval: function (min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    otherLang: function () {
      return (this.lang == 'pl' ? 'en' : 'pl');
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
      let otherLang = this.otherLang();
      this.correct = word[otherLang];
      let anwsers = [word[otherLang]];

      for (let i = 0; i < 3; i++) {
        rand = this.randomIntFromInterval(0, tmpWords.length - 1);
        anwsers.push(tmpWords[rand][otherLang]);
        console.log(tmpWords, tmpWords[rand], rand, otherLang);
        tmpWords.splice(rand, 1);
      }

      this.anwsers = this.shuffle(anwsers);
    },
    shuffle: function (array) {
      let currentIndex = array.length;

      // While there remain elements to shuffle...
      while (currentIndex != 0) {

        // Pick a remaining element...
        let randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;

        // And swap it with the current element.
        [array[currentIndex], array[randomIndex]] = [
          array[randomIndex], array[currentIndex]];
      }

      return array;
    },
    markAnswer(anwser) {
      if (anwser === this.correct) {
        this.$parent.addPoint(5);
        this.$parent.showAlert('success', 'Dobrze!');
      } else {
        this.$parent.showAlert('danger', 'Spróbuj jeszcze raz!');
      }

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
        <div class="col col-6 mt-4" v-for="anwser in anwsers">
          <a href="#" class="btn btn-primary w-100" @click="markAnswer(anwser)">{{ anwser }}</a>
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
.btn {
  font-size: 18px;
}
</style>