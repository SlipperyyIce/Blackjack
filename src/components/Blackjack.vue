<script lang="ts">
import { defineComponent } from 'vue'
    export default defineComponent({
      data() {
        return {
          cardValues: ["Ace", "2", "3", "4", "5", "6", "7", "8", "9", "10", "Jack", "Queen", "King"],
          cardSuits: ["Hearts", "Diamonds", "Clubs", "Spades"],
          deck: [{ }],
          playerCards: [],
          dealerCards: [],
          playerScore: 0,
          dealerScore: 0,
        }
      },
      methods: {
        // Handle a player hit
        hit() {
          this.giveCard(true);
        },

        // Handle a player stand
        stand() {
            while(this.dealerScore < 17)	this.giveCard(false);
        },

        giveCard(isplayer) {
          if (isplayer){
            this.playerCards.push(this.deck.pop());
            document.getElementById("player-cards").innerHTML += ", " + this.playerCards[this.playerCards.length - 1].value + " of " + this.playerCards[this.playerCards.length - 1].suit;
          }
          else {
            this.dealerCards.push(this.deck.pop());
            document.getElementById("dealer-cards").innerHTML += ", " + this.dealerCards[this.playerCards.length - 1].value + " of " + this.dealerCards[this.playerCards.length - 1].suit ; 
          }
          this.updateScore();
        },
        
        // Calculate the score for a hand of cards
        calculateScore(cards) {
          var score = 0;
          var hasAce = false;
          for (var i = 0; i < cards.length; i++) {
            var card = cards[i];
            if (card.value === "Ace") {
              hasAce = true;
              score += 11;
            } else if (card.value === "Jack" || card.value === "Queen" || card.value === "King") {
              score += 10;
            } else {
              score += parseInt(card.value);
            }
          }
          if (hasAce && score > 21) {
            score -= 10;
          }
          return score;
        },

        // Update the score and check for a win
        updateScore() {
          this.playerScore = this.calculateScore(this.playerCards);
          this.dealerScore = this.calculateScore(this.dealerCards);       
        },

        reset(){}


      },
      created() {
          // Create a deck of cards
          for (var i = 0; i < this.cardValues.length; i++) {
            for (var i2 = 0; i2 < this.cardSuits.length; i2++) {
              var card = {
                value: this.cardValues[i],
                suit: this.cardSuits[i2]
              };
              this.deck.push(card);
            }
          }
          
          // Shuffle the deck
          for (var i = 0; i < this.deck.length; i++) {
            var j = Math.floor(Math.random() * this.deck.length);
            var temp = this.deck[i];
            this.deck[i] = this.deck[j];
            this.deck[j] = temp;
          }

      }
    });
</script>

<template>

	<p id="player-cards"></p>
	<p id="dealer-cards"></p>
	<p id="player-score">Player: {{ playerScore }}</p>
	<p id="dealer-score">Dealer: {{ dealerScore }}</p>
	<button v-on:click="hit">Hit</button>
	<button v-on:click="stand">Stand</button>
	<button v-on:click="reset">Reset</button>

</template>

<style scoped>

</style>
