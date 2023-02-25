
<template>
  <div id ='table' class="table">
    <div class="hand">
    <span v-for="(card,i) in dealerCards">
      <img :src="card.image" class='dealer_img' :id="i" :style="{'z-index': 100 - i}"/>
    </span>
  </div>
  <div class="hand">
    <span v-for="(card,i) in playerCards">
      <img :src="card.image" class=""/>
    </span>
  </div>
  
    
  </div>
  <div>    
    <h1 id="player-score" class="text-center text-white">Player: {{ playerScore }}</h1>
    <h1 id="dealer-score" class="text-center text-white">Dealer: {{ dealerScore }}</h1>
  </div>
  <p id="player-cards"></p>
    <p id="dealer-cards"></p>
    
  <div>
    <button v-on:click="hit">Hit</button>
    <button v-on:click="stand">Stand</button>
    <button v-on:click="reset">Reset</button>
  </div>

</template>

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
          loop_i: 0,
          disable: false,
        }
      },
      methods: {
        // Handle a player hit
        hit() {
          if(!this.disable) this.giveCard(true);
        },

        Won(i){
          this.disable = true;

        },

        // Handle a player stand
        stand() {
          if(!this.disable){
            this.disable = true;
            this.showHidden(1);
            
            if(this.dealerScore < 17)this.Loop();
          }
        },
        //  Loop with delay 
        
        Loop() {         
          setTimeout(()=> {  
            
            if(this.dealerScore > 17){ 

              if(this.dealerScore >21)this.Won(-1);  //Dealer bust
              else if(this.playerScore == this.dealerScore) this.Won(0); //Blackjack draw
              else if(this.playerScore > this.dealerScore)  this.Won(1); //Player won
              else this.Won(-1); ; //Dealer won
               this.loop_i = 11;
              
              } //exit loop at >17
            
            this.loop_i++;                    //  increment the counter
            if ( this.loop_i < 12) {
              this.giveCard(false);           
              this.Loop();           
            }                       
          }, 2000)
        },

        giveCard(isplayer) {
          if (isplayer){
            this.playerCards.push(this.deck.pop());
            document.getElementById("player-cards").innerHTML += ", " + this.playerCards[this.playerCards.length - 1].value + " of " + this.playerCards[this.playerCards.length - 1].suit;
            this.updateScore();
          }
          else {
            this.dealerCards.push(this.deck.pop());
            document.getElementById("dealer-cards").innerHTML += ", " + this.dealerCards[this.dealerCards.length - 1].value + " of " + this.dealerCards[this.dealerCards.length - 1].suit ;
            if(this.dealerCards.length == 2){
              this.dealerCards[1].image = '../images/FaceDown.png';
              
            }
            else this.updateScore();
          }
          
        },

        //Reveals the hidden card
        showHidden(i){
          let img_loc = '../images/';
          img_loc = img_loc.concat(this.dealerCards[i].value, this.dealerCards[i].suit,".png");
          
          document.getElementById('1').className += ' reveal';
          setTimeout( ()=> {this.dealerCards[i].image = img_loc; this.updateScore();},500);
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
                    
          if(this.playerScore > 21)  this.Won(-1); //Player bust
          if(this.playerScore == 21) this.stand();
   
          
        },
        

        reset(){
          this.deck = [];
          this.playerCards = [];
          this.dealerCards = [];
          this.playerScore = 0;
          this.dealerScore = 0;
          this.loop_i = 0;
          this.disable = false;
          document.getElementById("player-cards").innerHTML = '';
          document.getElementById("dealer-cards").innerHTML = '';
          this.start();
            
        },

        start(){
          // Create a deck of cards
          for (var i = 0; i < this.cardValues.length; i++) {
            for (var i2 = 0; i2 < this.cardSuits.length; i2++) {
              let img_loc = '../images/';
              img_loc = img_loc.concat(this.cardValues[i],this.cardSuits[i2],".png")
              var card = {
                value: this.cardValues[i],
                suit: this.cardSuits[i2],
                image: img_loc,
                
              };
              this.deck.push(card);
              console.log(card);
          
            }
          }
          
          // Shuffle the deck
          for (var i = 0; i < this.deck.length; i++) {
            var j = Math.floor(Math.random() * this.deck.length);
            var temp = this.deck[i];
            this.deck[i] = this.deck[j];
            this.deck[j] = temp;
          }
          this.disable = true;
          setTimeout( ()=> this.giveCard(true),500);
          setTimeout( ()=> this.giveCard(false),1500);
          setTimeout( ()=> this.giveCard(true),2500);
          setTimeout( ()=> this.giveCard(false),3500);
          
          setTimeout( ()=> this.disable = false,4000);

        }

      },
      created() {
          this.start()
      }
    });
</script>

<style scoped>
img{
  height: 201.6px;
  width: 144px;
  
  position: relative;
  animation: hit 1s  forwards;
  margin-left: -50px;
}
.dealer_img{
  animation: dealer_hit 1s  forwards;
}

.hand{
  height: 201.6px;
  display: flex;
}
@keyframes hit {
  0%   {left:-200px; opacity: 0.0;}
  100%  {left:100px; opacity: 1;}
}
@keyframes dealer_hit {
  0%   {left:400px; opacity: 0.0;}
  100%  {left:100px; opacity: 1;}
  
}
.reveal{
    animation: reveal_anim 1s forwards;
}
@keyframes reveal_anim {
  0%   {left:100px; }
  50%  {left:10px; }
  100%  {left:100px; }
  
}

.table{
  gap: 10px;
  height: 400px;
  
}
</style>
