<script setup>
import { ref } from 'vue';
import Accueil from './components/Accueil.vue'
import Play from './components/Play.vue';
import End from './components/EndOfGame.vue'

const screens = {
  "accueil": {
    id: "accueil",
    component: Accueil
  },
  "play": {
    id: "play",
    component: Play
  },
  "end": {
    id: "end",
    component: End
  }
};
const currentScreen = ref("accueil")

const players = ref([])

// To launch the game after adding the players
function play(playersValidated) {
  if (playersValidated.length > 0) {
    players.value = playersValidated
    currentScreen.value = "play"
  }
}

function updateTotalScore(totalScore, player) {
  players.value.find(e => e.name == player.name).totalScore = totalScore
}

function endGame() {
  players.value.sort((a, b) => b.totalScore - a.totalScore)
  currentScreen.value = "end"
}

function newGame() {
  players.value = []
  currentScreen.value = "accueil"
}
</script>

<template>
  <main>
    <template v-for="(screen) of screens">
      <div v-show="screen.id == currentScreen" class="componentContainer">
        <component :is="screen.component" @play="play" :players="players" :currentScreen="currentScreen" @updateTotalScore="updateTotalScore" @backToAccueil="currentScreen = 'accueil'" @endGame="endGame()" @newGame="newGame()">
        </component>
      </div>
    </template>
  </main>
</template>

<style scoped>
main {
  height: 100%;
}

.componentContainer {
  height: 100%;
}
</style>
