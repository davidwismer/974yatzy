<template>
    <div class="playContainer">
        <img class="backIcon" src="src/assets/back_icon.png" @click="$emit('backToAccueil')">
        <div class="scoreboardContainer">
            <scoreboard :player-names="shortenNames" :players="props.players" @update-total-score="updateTotalScore">
            </scoreboard>
            <div class="showResultsContainer">
                <button class="showResults" :class="{ disabled: !gameEnd }" @click="endGame">Voir les résultats</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed, ref } from "vue"
import Scoreboard from "./Scoreboard.vue"

const props = defineProps({
    players: {
        type: Array,
        required: true
    }
})

const emits = defineEmits(["updateTotalScore", "endGame", "backToAccueil"])

const gameEnd = ref(false)

function updateTotalScore(totalScore, player) {
    emits("updateTotalScore", totalScore, player)
    let allScoreValidated = true
    props.players.forEach(player => {
        if(player.totalScore == 0) allScoreValidated = false
    })
    gameEnd.value = allScoreValidated
}

const shortenNames = computed(() => {
    let shortNames = []
    props.players.forEach(player => {
        let shortName = player.name.substring(0, 3)
        shortNames.push(shortName.charAt(0).toUpperCase() + shortName.slice(1))
    })
    return shortNames
})

function endGame(){
    gameEnd.value = false
    emits("endGame")
}

</script>

<style scoped>
.playContainer {
    height: 100%;
    display: flex;
    flex-direction: column;
    overflow-y: hidden;
}
.scoreboardContainer {
    margin-left: 30px;
}

.backIcon {
    width: 30px;
    height: 30px;
    margin: 20px 20px 10px 30px;
}

.showResults {
    margin-top: 10px;
    border-radius: 25px;
    border: none;
    padding: 5px 10px;
    font-weight: 600;
    font-size: 16px;
    background-color: #0A210F;
    color: white;
}
.showResultsContainer {
    display: flex;
    justify-content: center;
}

.disabled {
    display: none;
}
</style>