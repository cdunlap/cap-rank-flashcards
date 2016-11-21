<template lang="jade">
  #app.container
    #welcome.col-xs-12(v-if="!started")
      h1.page-header Welcome
      p.lead These flashcards will assist in memorizing member ranks in Civil Air Patrol.
      p #[strong How to play]
        ol
          li When flashcard appears, decide on your answer (either out loud or in your head)
          li Click &quot;Reveal Answer&quot; to see the correct answer
          li Click &quot;Next Card&quot; to proceed to the next flashcard
      p Each rank will appear at random until each rank has been shown, then it will start over.
      button.btn.btn-lg.btn-block.btn-primary(v-on:click="start") Begin
    #flashcards.col-xs-12(v-if="started")
      flashcard(:card-data="currentCardData" @nextButtonClicked="nextCard")
</template>

<script>
import Flashcard from './Flashcard.vue'
import $ from 'jquery'

export default {
  name: 'app',
  components: { Flashcard },
  data () {
    return {
      started: false,
      cardData: [],
      currentCardData: null,
      currentCardIdx: 0
    }
  },
  beforeMount () {
    $.getJSON('static/data.json', data => {
      this.cardData = data;

      data.forEach(d => {
        // Preload the image
        const img = new Image();
        img.src = d.image;
      });
    });
  },
  methods: {
    shuffleCards: function () {
      const cardData = this.cardData;
      for (var i = cardData.length - 1; i > 0; i--) {
          var j = Math.floor(Math.random() * (i + 1));
          var temp = cardData[i];
          cardData[i] = cardData[j];
          cardData[j] = temp;
      }
      this.cardData = cardData;
    },
    start: function () {
      this.shuffleCards();
      this.started = true;
      this.currentCardIdx = 0;
      this.currentCardData = this.cardData[this.currentCardIdx];
    },
    nextCard: function () {
      this.currentCardIdx++;
      if(this.currentCardIdx >= this.cardData.length) {
        this.start(); // Start over
      } else {
        this.currentCardData = this.cardData[this.currentCardIdx];
      }
    }
  }
}
</script>

<style>

</style>
