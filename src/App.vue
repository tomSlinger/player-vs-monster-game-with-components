<template>
  <div id="app">

    <!--Player and Monster Section-->
    <section class="row">
      <!--Player-->
      <player :playerhealth="playerHealth" :playercolor="playerColor"></player>
      <!--Monster-->
      <monster :monsterhealth="monsterHealth" :monstercolor="monsterColor"></monster>
    </section>

    <!--Controls-->
    <section class="row controls">
      <controls :gameStatus="gameStarted" :attackFn="attackRound" :specAttackFn="specialAttackRound" :healFn="healRound" :giveUpFn="giveUp" :startGameFn="startNewGame"></controls>
    </section>

    <!--Log-->
    <section class="row log">
      <log :log="logs"></log>
    </section>

  </div>
</template>

<script>
  //Imports
  import Player from './components/Player.vue';
  import Monster from './components/Monster.vue';
  import Controls from './components/Controls.vue';
  import Log from './components/Log.vue';

  import { eventBus } from './main.js';

  export default {
    data(){
      return {
        gameStarted: false,
        playerHealth: 100,
        playerColor: 'green',
        monsterHealth: 100,
        monsterColor: 'green',
        logs: []
      }
    },
    methods: {
      startNewGame(){
        this.gameStarted = !this.gameStarted;
        this.playerHealth = 100;
        this.monsterHealth = 100;
        this.logs = [];
      },
      attackRound(){
        var playerDmg = Math.floor((Math.random() * 10) + 1);
        this.monsterHealth -= playerDmg;
        this.logs.unshift({
          message: "Player hurt the Monster for " + playerDmg + " damage using a Standard Attack"
        });

        this.monsterRound();

        this.checkVictoryStatus();
      },
      specialAttackRound(){
        var playerDmg = Math.floor((Math.random() * 10) + 5);
        this.monsterHealth -= playerDmg;
        this.logs.unshift({
          message: "Player hurt the Monster for " + playerDmg + " damage using a Special Attack."
        });

        this.monsterRound();

        this.checkVictoryStatus();
      },
      healRound(){
        var playerHeal = 10;
        if(this.playerHealth + playerHeal > 100){
          playerHeal = 100 - this.playerHealth;
          this.playerHealth += playerHeal;
        }else{
          this.playerHealth += playerHeal;
        }
        this.logs.unshift({
          message: "Player healed " + playerHeal + " points of health."
        });

        this.monsterRound();

        this.checkVictoryStatus();
      },
      giveUp(){
        if(confirm("You ran away like a little girl... Try Again?")){
          this.startNewGame();
        }else{
          this.gameStarted = false;
        }
      },
      monsterRound(){
        var monsterDmg = Math.floor((Math.random() * 15) + 1);
        this.playerHealth -= monsterDmg;
        this.logs.unshift({
          message: "Monster hurt the Player for " + monsterDmg + " damage."
        });
      },
      checkVictoryStatus(){
        if(this.monsterHeath <= 0){
          if(confirm("Congrats, you defeated the monster! Start a new game?")){
            this.startNewGame();
          }else{
            this.gameStarted = false;
          }
        };
        if(this.playerHealth <= 0){
          if(confirm("Oh no! The monster killed you! Start a new game?")){
            this.startNewGame();
          }else{
            this.gameStarted = false;
          }
        }
      }
    },
    components: {
      Player,
      Monster,
      Controls,
      Log
    },
    created(){
      eventBus.$on('gameHasStarted', (gameStatus) => {
        this.gameStarted = gameStatus;
      });
      eventBus.$on('clearLog', (log) => {
        this.logs = log;
      });
      eventBus.$on('resetPlayerHealth', (health) => {
        this.playerHealth = health;
      })
      eventBus.$on('resetMonsterHealth', (health) => {
        this.monsterHealth = health;
      })
    }
  }
</script>

<style>
.text-center {
    text-align: center;
}

.healthbar {
    width: 100%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
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

.log ul li:nth-child(odd){
    color: red;
    background-color: #ffc0c1;
}

.log ul li:nth-child(even){
    color: blue;
    background-color: #e4e8ff;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color: #76ff7e;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ffffff;
}

#give-up:hover {
    background-color: #c7c7c7;
}
</style>
