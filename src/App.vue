<template>
  <div id="app">
    <header>
      <h1>Память</h1>
    </header>
    <Timer :time="formatTime(currentTime)" />
    <div class="current-rating" v-if="currentRating !== 0"><small> Ваше место в рейтинге: {{currentRating}}</small></div>
    <Button class="start-btn" v-on:click="startGame">Старт</Button>
    <div class="main-container">
      <div class="game">
       
        <CardsField v-on:card-clicked="checkCard($event)" :cards="cards" />
      </div>
      <div class="rating">
        <RatingField :ratings="gameResults" />
      </div>
    </div>
  </div>
</template>

<script>
import CardsField from "./components/CardsField.vue";
import Timer from "./components/Timer.vue";
import RatingField from "./components/RatingField.vue";



export default {
  name: "app",
  data() {
    return {
      cards: this.randomizeCards([
        "face",
        "pets",
        "headset",
        "fingerprint",
        "http",
        "loyalty",
        "home",
        "change_history",
        "pan_tool",
        "motorcycle",
        "payment",
        "track_changes",
        "work",
        "visibility",
        "warning",
        "mic",
        "web",
        "pause",
        "face",
        "pets",
        "headset",
        "fingerprint",
        "http",
        "loyalty",
        "home",
        "change_history",
        "pan_tool",
        "motorcycle",
        "payment",
        "track_changes",
        "work",
        "visibility",
        "warning",
        "mic",
        "web",
        "pause",
      ]),
      currentTime: 0,
      startTime: 0,
      selectedCards: [],
      currentRating: 0,
      gameStared: false,
      deletedCards: [],
      
      gameResults: [],
      waiterTimeout:'',
      tempArr:[],
    };
  },
  methods: {
    startGame() {
      if (!this.gameStared) {
        document.querySelector('.start-btn').disabled = true
        document.querySelector('.start-btn').innerHTML = 'Удачной игры :)'
        document.querySelector('.start-btn').style.opacity = 0
        this.gameStared = true;
        this.startTime = new Date();
        this.currentTime = 0;
        this.countTimeInterval = setInterval(() => {
          this.countTime();
        }, 100);
      }
    },
    checkCard(cardIndex) {
      if (this.gameStared) {
        this.waiterTimeout = setTimeout(() => {
          if(cardIndex === this.selectedCards[0].cardIndex){
            this.clearSelectedCards()
          }
        }, 5000);
        if (this.selectedCards.length < 2) {
          
          this.selectedCards.length === 1 ? clearTimeout(this.waiterTimeout) : ''

          this.selectedCards.push({
            cardValue: this.cards[cardIndex],
            cardIndex: cardIndex
          });

          let selectedCard = document.querySelector(
            "div[data-key='" + cardIndex + "']"
          );

          selectedCard.classList.add("selected");
          selectedCard.innerHTML = "<i class='material-icons'>" + this.cards[cardIndex] + "</i>";

          if (this.selectedCards.length === 2) {
          
            if (this.selectedCards[0].cardIndex == cardIndex) {
              this.clearSelectedCards();
            } else if (
              this.selectedCards[0].cardValue ===
              this.selectedCards[1].cardValue
            ) {
              this.selectedCards.forEach(card => {
                document
                  .querySelector("div[data-key='" + card.cardIndex + "']")
                  .remove();
                this.deletedCards.push(card);
                if (this.deletedCards.length === this.cards.length) {
                  this.gameStared = false;
                  clearTimeout(this.countTimeInterval);
                  this.addGameResult(this.currentTime);
                  this.getGameResults()
                }
              });
            }
            setTimeout(() => {
              this.clearSelectedCards();
            }, 200);
          }
        }
      }
    },

    addGameResult(result) {
      localStorage.setItem(`game_${localStorage.length+1}`, JSON.stringify({result:result,date:new Date().toUTCString()}));
    },

    getGameResults() {
      this.gameResults = []
      for(let i=0; i<localStorage.length; i++) {
        let key = localStorage.key(i);
        let value = localStorage.getItem(key)
        if(key.includes('game'))
          this.gameResults.push(value)
      } 
    },

    clearSelectedCards() {

    

      this.selectedCards.forEach(card => {
        let cardContainer = document.querySelector(
          "div[data-key='" + card.cardIndex + "']"
        );
        if (cardContainer != null) {
          cardContainer.classList.remove("selected");
          cardContainer.innerHTML = "";
        }
      });

      this.selectedCards = [];
    },

    getCurrentRating(currentTime){

      let tempArr = []
      this.gameResults.forEach(res =>{
        let json = JSON.parse(res)
         tempArr.push(json)
      })
      tempArr.push({result:currentTime,date:new Date().toUTCString()})
      tempArr.sort(function (a, b) {
        if (a.result > b.result) {
          return 1;
        }
        if (a.result < b.result) {
          return -1;
        }
        // a должно быть равным b
        return 0;
      });
      this.tempArr = tempArr

      let pos = tempArr.map(function(e) { return e.result; }).indexOf(currentTime);

      return pos + 1 

    },

    countTime() {
      this.currentRating = this.getCurrentRating(this.currentTime)
      this.currentTime = (new Date() - this.startTime);
    },
    formatTime(millis) {
      let minutes = Math.floor(millis / 60000);
      let seconds = ((millis % 60000) / 1000).toFixed(0);
      return minutes + ":" + (seconds < 10 ? '0' : '') + seconds;

    },


    randomizeCards(array) {
      var currentIndex = array.length,
        temporaryValue,
        randomIndex;
      while (0 !== currentIndex) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    }
  },
  mounted: function(){
    this.getGameResults()
  },
  components: {
    CardsField,
    Timer,
    RatingField
  }
};
</script>

<style lang="scss">

$base-color:  '#03a9f4';

body {
  margin: 0;
}
#app {
  padding: 5px;
  font-family: "Roboto", "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: #2c3e50;
}
header{
  height: 50px;
}
.main-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
h1{
  padding-left: 10px;
}
button {
  border: none;
  padding: 5px;
  margin: 5px;
}
.start-btn{
  text-align:center;
  width:150px;
  justify-self: center;
  align-self: center;
  cursor: pointer;
}
.current-rating{
  opacity: 0.5;
  text-align: center;
}
@font-face {
  font-family: "Roboto";
  src: url("assets/fonts/Roboto-Light.ttf") format("ttf");
}

@font-face {
  font-family: 'Material Icons';
  font-style: normal;
  font-weight: 400;
  src: url(assets/fonts/MaterialIcons-Regular.eot); /* For IE6-8 */
  src: local('Material Icons'),
    local('MaterialIcons-Regular'),
    url(assets/fonts/MaterialIcons-Regular.woff2) format('woff2'),
    url(assets/fonts/MaterialIcons-Regular.woff) format('woff'),
    url(assets/fonts/MaterialIcons-Regular.ttf) format('truetype');
}

.material-icons {
  font-family: 'Material Icons';
  font-weight: normal;
  font-style: normal;
  font-size: 24px;  /* Preferred icon size */
  display: inline-block;
  line-height: 1;
  text-transform: none;
  letter-spacing: normal;
  word-wrap: normal;
  white-space: nowrap;
  direction: ltr;

  /* Support for all WebKit browsers. */
  -webkit-font-smoothing: antialiased;
  /* Support for Safari and Chrome. */
  text-rendering: optimizeLegibility;

  /* Support for Firefox. */
  -moz-osx-font-smoothing: grayscale;

  /* Support for IE. */
  font-feature-settings: 'liga';
}
@media only screen and (max-width: 1000px) {
  button{
    
    width: 80vw!important;
    font-size: 3vh;

    margin:10px
  }
}
</style>
