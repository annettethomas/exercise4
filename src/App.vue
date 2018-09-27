<template>
  <div id="app">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">PLAYER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;" :style="{ width: playerPoint + '0%' }">
            {{ playerPoint }}
          </div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">COMPUTER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;" :style="{ width: computerPoint + '0%' }">
            {{ computerPoint }}
          </div>
        </div>
      </div>
    </section>
    <section class="row controls">
      <div class="small-12 columns">
        <button id="start-game" @click="startGame">START NEW GAME</button>
      </div>
    </section>
    <section class="row controls" v-show="show">
      <div class="small-12 columns">
        <button id="rock" @click="rockChoice">ROCK</button>
        <button id="paper" @click="paperChoice">PAPER</button>
        <button id="scissors" @click="scissorsChoice">SCISSORS</button>
        <button id="restart" @click="restartGame">RESTART</button>
      </div>
    </section>
    <section class="row log" v-show="show">
      <div class="small-12 columns">
        <ul>
          <li v-for="turn in turns" :class="{'player-turn': turn.isPlayer,
        'computer-turn': !turn.isPlayer, 'tie-turn': turn.tie}">
            {{ turn.text }}</li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      playerPoint: 0,
      computerPoint: 0,

      show: false,

      compChoice: ['rock', 'paper', 'scissors'],
      turns: []
    }
  },

  methods: {

    startGame(){
      this.show = true
      this.playerPoint = 0
      this.computerPoint = 0
      this.turns = []
    },

    restartGame(){
      this.show = false
      this.playerPoint = 0
      this.computerPoint = 0
    },
    rockChoice: function(){

      let comp = this.computerChoice();
      if(comp === 'paper'){
        this.computerPoint += 1;
        this.turns.unshift({
          isPlayer: false,
          text: 'Computer Chose Paper | Paper Beats Rock | Computer Wins'
        })
      }else if(comp === 'scissors'){
        this.playerPoint += 1;
        this.turns.unshift({
          isPlayer: true,
          text: 'Computer Chose Scissors | Rock Beats Scissors | Player Wins'
        })
      }else {
        this.turns.unshift({
          tie: true,
          text: 'Computer Chose Rock | You Have Tied!'
        })

      }

      this.checkWinner();

    },

    paperChoice: function(){

      let comp = this.computerChoice();
      if(comp === 'scissors'){
        this.computerPoint += 1;
        this.turns.unshift({
          isPlayer: false,
          text: 'Computer Chose Scissors | Scissors Beats Paper | Computer Wins'
        })
      }else if(comp === 'rock'){
        this.playerPoint += 1;
        this.turns.unshift({
          isPlayer: true,
          text: 'Computer Chose Rock | Paper Beats Rock | Player Wins'
        })
      }else {
        this.turns.unshift({
          tie: true,
          text: 'Computer Chose Paper | You Have Tied!'
        })
      }
      this.checkWinner();
    },

    scissorsChoice: function() {

      let comp = this.computerChoice();
      if(comp === 'rock'){
        this.computerPoint += 1;
        this.turns.unshift({
          isPlayer: false,
          text: 'Computer Chose Rock | Rock Beats Scissors | Computer Wins'
        })
      }else if(comp === 'paper'){
        this.playerPoint += 1;
        this.turns.unshift({
          isPlayer: true,
          text: 'Computer Chose Paper | Scissors Beats Paper | Player Wins'
        })
      }else {
        this.turns.unshift({
          tie: true,
          text: 'Computer Chose Scissors | You Have Tied!'
        })
      }

      this.checkWinner();
    },

    computerChoice: function(){
      let vm = this;
      let computer = vm.compChoice[Math.floor(Math.random() * vm.compChoice.length)]
      return computer

    },

    checkWinner: function(){

      if(this.playerPoint === 10){
        if(confirm('You won! Restart?')){
          this.startGame();
        } else {
          this.show = false;
        }
      }else if(this.computerPoint === 10){
        if(confirm('You lost! Rematch?')){
          this.startGame();
        } else {
          this.show = false;
        }
      }

    }

  }
}
</script>

<style>
  .text-center {
    text-align: center;

  }

  .wins {
    width: 80%;
    color: black;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 1000ms;

  }

  .controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
  }

  .turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
  }

  .log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
  }

  .log ul li {
    margin: 5px;
  }

  /*PLAYER WIN*/
  .log ul .player-turn {
    color: green;
    background-color: #aaffb0;
  }

  /*COMPUTER WIN*/
  .log ul .computer-turn {
    color: red;
    background-color: #ffc0c1;
  }

  /*TIE WIN*/
  .log ul .tie-turn {
    color: blue;
    background-color: #e4e8ff;
  }

  button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
  }

  #start-game {
    background-color: #e4e8ff;
  }

  #start-game:hover {
    background-color: #687eff;
  }

  #rock {
    background-color: #ff7367;
  }

  #rock:hover {
    background-color: #ff3f43;
  }

  #paper {
    background-color: #ffaf4f;
  }

  #paper:hover {
    background-color: #ff9a2b;
  }

  #scissors {
    background-color: #aaffb0;
  }

  #scissors:hover {
    background-color: #76ff7e;
  }

  #restart {
    background-color: #ffffff;
  }

  #restart:hover {
    background-color: #c7c7c7;
  }
</style>
