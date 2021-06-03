<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players 
        v-bind:isWinner="isWinner"
        v-bind:scoresPlayer="scoresPlayer"
        v-bind:activePlayer="activePlayer"
        v-bind:currentScore="currentScore"
      />
      <controls
        v-bind:finalScore="finalScore"
        v-on:handleChangeFinalScore="handleChangeFinalScore"
        v-on:handleHold="handleHold"
        v-on:handleNewGame="handleNewGame"
        v-on:handleRollDice="handleRollDice"
      /> 
      <dices
        v-bind:dices="dices"
      /> 
      <popup-rule
        v-on:handleConfirm="handleConfirm"
        v-bind:isOpenPopup="isOpenPopup"
      />   
    </div>
  </div>
</template>

<script>
import Players from './components/Players'
import Controls from './components/Controls'
import Dices from './components/Dices'
import PopupRule from './components/PopupRule'
export default {
  name: 'app',
  data () {
    return {
      isPlaying: false,
      isOpenPopup: false,
      activePlayer:0,
      scoresPlayer: [0,0],
      dices: [5,5],
      currentScore: 0,
      finalScore: 10
    }
  },
  components:{
    Players,
    Controls,
    Dices,
    PopupRule
  },
  computed:{
    isWinner(){
      let {finalScore, scoresPlayer} = this;
      if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >=finalScore ){
        this.isPlaying = false;
        return true;
      }
      return false;
    }
  },
  methods: {
    handleChangeFinalScore(e){
      var number = parseInt(e.target.value);
      if(isNaN(number)){
        this.finalScore= '';
      }else{
        this.finalScore = number;
      }
    },
    handleHold(){
      if(this.isPlaying){
        let {scoresPlayer,activePlayer,currentScore} = this;
        let scoresOld = scoresPlayer[activePlayer];
        // let cloneScorePlayer = [...scoresPlayer];
        //     cloneScorePlayer[activePlayer]= scoresOld + currentScore;
        // this.scoresPlayer = cloneScorePlayer;
        this.$set(this.scoresPlayer,activePlayer,scoresOld + currentScore);
        if(!this.isWinner){
          this.nextPlayer();
        }
        
      }else{
        alert('Vui lòng ấn vào nút New Game')
      }
    },
    nextPlayer(){
      this.activePlayer= this.activePlayer ===0 ? 1 :0;
      this.currentScore = 0;
    },
    handleConfirm(){
      this.isPlaying = true;
      this.dices = [1,1];
      this.isOpenPopup = false;
      this.activePlayer = 0;
      this.scoresPlayer = [0,0];
      this.currentScore = 0;

    },
    handleNewGame(){
      this.isOpenPopup = true;
    },
    handleRollDice(){
      console.log('Handle vue Roll')
      if(this.isPlaying){
        //Xoay xúc xắc
        var dice1 = Math.floor(Math.random()*6+1);
        var dice2 = Math.floor(Math.random()*6+1);

        this.dices = [dice1,dice2];

        if(dice1===1 || dice2 ===1){
          let activePlayer = this.activePlayer
          setTimeout(function(){
            alert(`Người chơi Player ${activePlayer +1 } đã quay trúng ô số 1 .Rất tiếc`)
          },10)
          this.nextPlayer();
        }else{
          this.currentScore=this.currentScore + dice1+dice2;
        }


        console.log(dice1,dice2)
      }else{
        alert('Vui lòng ấn vào nút New Game')
      }
    }
  },
}
</script>

<style>
/**********************************************
*** GENERAL
**********************************************/

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}


</style>
