
<template>
<div class="container">
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
  <div class="Scores">    
    <h1 id="dealer-score" class=" text-white pixel_font" style="margin-bottom: 150PX;">Dealer: {{ dealerScore }}</h1>
    <h1 id="player-score" class=" text-white pixel_font" style="margin-bottom: 150PX;" >Player: {{ playerScore }}</h1>

    <button v-on:click="hit">Hit</button>
    <button v-on:click="stand">Stand</button>
    <button v-on:click="reset">Reset</button>
    
    <h1 id="player-score" class=" text-white pixel_font" >Blackjack Overview</h1>
    <div style="display: flex; gap: 150px;">
    <p  class=" text-white pixel_font" style="font-size: 20px; width:300px;"> Welcome to a free online blackjack game I made using javascript so you can gamble to your hearts content with no risk!</p>
    <p  class=" text-white pixel_font" style="font-size: 20px; width:500px;">The rules are simple if you win if your combined value of cards is less than 22 or greater then the dealers total card value</p>
 
    </div>
    <button v-on:click="toggleMusic">Background Music: {{ music_active }}</button>

    <div class="modal" tabindex="-1" role="dialog" id="myModal">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Modal title</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <p>Modal body text goes here.</p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-primary">Save changes</button>
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
          </div>
        </div>
      </div>
    </div>
    

  </div>
 
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
          timeouts:[],
          music_active: "Off",
          bgMusic: document.createElement("audio"),
          
        }
      },
      methods: {
        // Handle a player hit
        hit() {
          if(!this.disable) this.giveCard(true);
        },

        Won(i){
          this.disable = true;

          
          switch (i){
            case -1:
              alert("U LOSE");
              break;

            case 0:
              alert()
              break;

            case 1:
              alert()
              break;
          
          }

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
          let t = setTimeout(()=> {  
            
            if(this.dealerScore > 17){ 

              if(this.dealerScore >21)this.Won(1);  //Dealer bust
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
          this.timeouts.push(t);
        },

        giveCard(isplayer) {
          if (isplayer){
            this.playerCards.push(this.deck.pop());
            this.updateScore();
          }
          else {
            this.dealerCards.push(this.deck.pop());
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
          if(this.dealerCards.length == 2){
            if(this.dealerCards[1].image == '../images/FaceDown.png'){                
                }
            else this.dealerScore = this.calculateScore(this.dealerCards);                      
          }
          else this.dealerScore = this.calculateScore(this.dealerCards);
          
          
             
                    
          if(this.playerScore > 21)  this.Won(-1); //Player bust
          if(this.playerScore == 21) this.stand();
   
          
        },
        
        toggleMusic() {
          if(this.bgMusic.paused) {
            this.bgMusic.play();
            this.music_active = "On ";

          }

          else {
            this.bgMusic.pause();
            this.music_active = "Off";
          }
        },

        reset(){
          this.deck = [];
          this.playerCards = [];
          this.dealerCards = [];
          this.playerScore = 0;
          this.dealerScore = 0;
          this.loop_i = 0;
          this.disable = false;
          for (var i = 0; i < this.timeouts.length; i++) {
            clearTimeout(this.timeouts[i]);
          }
          this.timeouts = [];
          
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

          let t = setTimeout( ()=> this.giveCard(true),500);
          this.timeouts.push(t);
          t = setTimeout( ()=> this.giveCard(false),1500);
          this.timeouts.push(t);
          t = setTimeout( ()=> this.giveCard(true),2500);
          this.timeouts.push(t);
          t = setTimeout( ()=> this.giveCard(false),3500);
          this.timeouts.push(t); 
          t =setTimeout( ()=> this.disable = false,4000);
          this.timeouts.push(t);
          
        }

      },
      created() {
          this.start()
          
          this.bgMusic.src = '../music/swing.mp3';
          this.bgMusic.loop = true;
          this.bgMusic.setAttribute("preload", "auto");
          this.bgMusic.setAttribute("controls", "none");
          this.bgMusic.style.display = "none";
          document.body.appendChild(this.bgMusic);
          
          
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
.btn_div{
  position: relative;
  top: -230px;
    left: 58px;
}
.dealer_img{
  animation: dealer_hit 1s  forwards;
}
.pixel_font{
  font-family: "REDENSEK";
}

.Scores{
  position: relative;
  top: -350px;
  right: 150px;
}

.container{
  position: relative;
  left: 150px;
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
