<template>
<v-app>
 <div id="app">
    <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">YOU</h1>
            <div class="healthbarMe">
                <div class="healthbarMe text-center" :style="{width: myHealth + '%'}" style="background-color: green; margin: 0; color: white;">
                    {{ myHealth }}
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">MONSTER</h1>
            <div class="healthbarMonster">
                <div class="healthbarMonster text-center" :style="{width: monsterHealth + '%'}" style="background-color: green; margin: 0; color: white;">
                    {{ monsterHealth }}
                </div>
            </div>
        </div>
    </section>
    <section v-if="!gameOn"  class="row controls">
        <div class="small-12 columns">
              <v-btn color="success" @click="newGame" id="start-game">START NEW GAME</v-btn>
        </div>
    </section>
    <section  v-else class="row controls">
        <div  class="small-12 columns">
           <v-btn color="red" id="attack" @click="attack">ATTACK</v-btn>
             <v-btn color="warning" id="special-attack" @click="specialAttack">SPECIAL ATTACK</v-btn>
             <v-btn color="success" id="heal" @click="healing">HEAL</v-btn>
            <v-btn color="info" @click="giveUp"  id="give-up">GIVE UP</v-btn>
        </div>
    </section>
    <section class="row log" v-if="attackLog.length > 0">
        <div class="small-12 columns">
            <h2 class="monster-turn" :style="{background: turnBackcolor, color: turnColor}">{{  monsterAttachHere }}</h2>
            <h2 class="player-turn" :style="{background: turnBackcolor, color: turnColor}">{{ myAttackHere }}</h2>

            <ul>
                <li v-for="attack in attackLog" v-bind:key="attack.id" :class="{'player-turn': attack.isPlayer, 'monster-turn': !attack.isPlayer}"> 
                {{ attack.text }}
            
                </li>
            </ul>            
        </div>
    </section>
</div>
</v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  data: () => ({
        gameOn: false,
        myHealth : 100,
        monsterHealth : 100,
        attackLog: [],
        myAttackHere : `En garde! `,
        monsterAttachHere : `ROAR`,
        turnColor: 'black',
        turnBackcolor : 'yellow'
         }),

    methods:{

        monsterAttack: function(myHealth){
           let hitMe =  Math.floor(Math.random()*10+4)
           this.myHealth = this.myHealth - hitMe
           this.attackLog.unshift({
            isPlayer: false,
            text:  `Monster Attack : ${hitMe}`})
       
           this.monsterAttachHere = (`Monster Attack : ${hitMe}`)
        
    
        },
 
        myAttack: function(monsterHealth){
            let hitMonster = Math.floor(Math.random()*10+1)
            this.monsterHealth = this.monsterHealth - hitMonster
            this.attackLog.unshift({
                isPlayer: true,
                text: `My Attack : ${hitMonster}`})
            this.myAttackHere = ( `My Attack : ${hitMonster}`)
         
         
        },

        mySpecialAttack: function(monsterHealth){
            let hitMonsterSpecial = Math.floor(Math.random()*10+6)
            this.monsterHealth = this.monsterHealth - hitMonsterSpecial
            this.attackLog.unshift({
                isPlayer: true,
                text :`My Special Attack : ${hitMonsterSpecial}`})
            this.myAttackHere = (`My Special Attack : ${hitMonsterSpecial}`)
            
        },

        healMe: function(myHealth){
            if (this.myHealth < 94){
            let moreHealth = Math.floor(Math.random()*10+3)
            this.myHealth = this.myHealth + moreHealth
            this.attackLog.unshift({
                isPlayer: true,
                text: `Healing : ${moreHealth}`})
            this.myAttackHere = (`Healing : ${moreHealth}`)

            }else {this.myHealth = 100}
        },

        checkLife: function(){
            if(this.monsterHealth<= 0){
                this.myAttackHere = (`You win!  The Monster is dead!  Game Over`)
                this.gameOn = false
                this.turnColor = '#add8e6',
                this.turnBackcolor = 'black'
            
        }else if(this.myHealth<=0){
                this.monsterAttachHere = (`You are dead!  Game Over`)
                this.gameOn = false
                this.turnColor = 'black',
                this.turnBackcolor = 'red'
             }
        },

        attack: function(){
            this.monsterAttack()
            this.myAttack()
            this.checkLife()
       
        },

        specialAttack: function(){
            this.monsterAttack()
            this.mySpecialAttack()
            this.checkLife()
        },

        healing: function(){
            this.monsterAttack()
            this.healMe()
            this.checkLife()
        
        },

        newGame: function(){
            this.myHealth = 100
            this.monsterHealth = 100
            this.gameOn = true
            this.myAttackHere = `En garde! `,
            this.monsterAttachHere = `ROAR!`,
            this.attackLog = []
            this.turnColor ='black',
            this.turnBackcolor = 'yellow'
        },

        giveUp: function(){
            this.gameOn = false;
            this.monsterAttachHere = (`You gave up!`)
            this.myAttackHere = (`Game Over`)
            this.turnColor = 'white',
            this.turnBackcolor = 'purple'
        }
    }
};
</script>

<style>
#app{ 
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin: 0 auto;
}

  .text-center {
    text-align: center;
}

.healthbarMe {
    width: 100%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
    border: 1px solid black;
}

.healthbarMonster {
    width: 100%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
    border: 1px solid black;
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
