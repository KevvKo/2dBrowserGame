<template>
    <router-view/>
</template>

<style>

  /*variables for uniform design*/
  :root{

      /*general styling */
      --default-transition: 0.3s; 
      --default-body-color: #E1E2E1;
      --default-font: 'Sriracha';

      /*primary color */
      --main-primary-color: #447a6d;
      --main-primary-txt: white;
      --main-primary-opacity: 93%;

      /*light color */
      --main-light-color: #72a99b;
      --main-light-txt: black;
      --main-light-opacity: 85%;
      --main-light-shadow: 3px 3px white;

      /*dark color */
      --main-dark-color: #154e42;
      --main-dark-txt: white;
      --main-dark-opacity: 60%;
  }

  /*general styling*/

  *{
      box-sizing: border-box;
  }

  html, body{
      height: 100%;
      margin: 0px;
      padding: 0px;
  }

  body {
      font-family: var(--default-font);
      background-color: var(--default-body-color)

  }

  #app{
      height: 100%;
      display: flex;
      flex-direction: column;
  }

  button{
      box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19)
  }
</style>

<script>

  import Vue from 'vue'
  import Vuex from 'vuex'

  //classes imports
  import Game from './assets/js/game.js'
  import Score from './assets/js/score'
  import DbHandler from './assets/js/dbHandler'

  Vue.use(Vuex)

  const store = new Vuex.Store({

    state: {

        grayoutVisible: false,

        colorVisible: false,

        highscoreBoardVisible: false,
        showGameOver: false,

        //modulo 20 for the stepWidth fromthe snake
        width: (window.innerWidth * 0.8) - (window.innerWidth * 0.8)%20,
        height: (window.innerHeight * 0.8) -(window.innerHeight * 0.8)%20,

        game: new Game(),
        scoreHandler: new Score(),
        db: new DbHandler(),

        choosenColor: 'green',      //default
        colorSets: {

            green: {
            
                primary: '#038c00',
                light: '#50bd3d',
                dark: '#005d00'
            },

            blue:{
                
                primary: '#00639e',
                light: '#4f90cf',
                dark: '#00396f'
            },
            
            red: {
                
                primary: '#a30000',
                light: '#dc462b',
                dark: '#6d0000'
            }
        } 

    },

    mutations: {

        changeVisibility (state) {

            state.showMenu = false
        },

        checkHighScore(state){

            if(state.db.getPlayerStats()){
                state.scoreHandler.highscore = state.db.getHighScore()
            }
        },

        finisheGame (state){

            state.showGameOver = true
            state.game.clearCanvas()
        },

        resetGame(state){
            state.game = new Game()
            state.scoreHandler = new Score()
        }
    },
  })

  window.addEventListener('resize', function(){
      let newWidth = window.innerWidth * 0.8
      store.state.width = newWidth
  })

  export {store} 

  export default {
    
    name: 'App',

    store
  }

</script>

