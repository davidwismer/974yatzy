<template>
    <div class="endOfGameContainer">
        <h1 class="title">Résultats!</h1>
        <div class="podiumContainer">
            <div class="podium2e">
                <div class="playerScoreContainer">
                    <span v-if="props.players[1]" class="playerName">{{ props.players[1].name }}</span>
                    <span v-if="props.players[1]" class="playerScore">{{ props.players[1].totalScore }}</span>
                </div>
                <span class="place two">2</span>
            </div>
            <div class="podium1e">
                <div class="playerScoreContainer">
                    <span v-if="props.players[0]" class="playerName">{{ props.players[0].name }}</span>
                    <span v-if="props.players[0]" class="playerScore">{{ props.players[0].totalScore }}</span>
                    <ConfettiExplosion v-if="activeclub300" class="club300" :particleCount="300" :duration="5000">
                    </ConfettiExplosion>
                </div>
                <span class="place one">1</span>
            </div>
            <div class="podium3e">
                <div class="playerScoreContainer">
                    <span v-if="props.players[2]" class="playerName">{{ props.players[2].name }}</span>
                    <span v-if="props.players[2]" class="playerScore">{{ props.players[2].totalScore }}</span>
                </div>
                <span class="place three">3</span>
            </div>
        </div>
        <div class="underPodiumContainer">
            <table class="restContainer">
                <tr v-for="(player, index) of props.players.slice(3)">
                    <td class="positionRest">{{ index + 4 }}e</td>
                    <td class="playerNameRest">{{ player.name }}</td>
                    <td class="playerScoreRest">{{ player.totalScore }}</td>
                </tr>
            </table>
            <div class="newGameContainer">
                <button class="newGame" @click="newGame" disabled>Nouvelle Partie</button>
            </div>
        </div>
    </div>
</template>

<script setup>
import { watchEffect, ref } from "vue"
import ConfettiExplosion from "vue-confetti-explosion";
const props = defineProps({
    players: {
        type: Array,
        required: true
    },
    currentScreen: {
        type: String
    }
})
const emits = defineEmits(['newGame'])
const activeclub300 = ref(false)

watchEffect(() => {
    if (props.currentScreen == "end") {
        setTimeout(() => {
            document.querySelector(".newGame").disabled = false
            if (props.players[0].totalScore >= 300) {
                activeclub300.value = true
            }
        }, 4000)
    }
})

function newGame() {
    document.querySelector(".newGame").disabled = true
    activeclub300.value = false
    emits('newGame')
}
</script>

<style scoped>
.endOfGameContainer {
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 2rem;
    overflow: hidden;
}

.title {
    text-align: center;
}

.podiumContainer {
    display: flex;
    justify-content: center;
    align-items: end;
    height: 180px;
    margin-top: 50px;
    margin-bottom: 20px;
}

.podium3e,
.podium2e,
.podium1e {
    background-color: #ededed;
    width: 30%;
}

.podium3e {
    height: 70px;
    transform: translateY(-100vh);
    animation: 1s ease-in 0s 1 slideAnimation;
    animation-fill-mode: forwards;
    z-index: 0;
}

.podium2e {
    height: 100px;
    transform: translateY(-100vh);
    animation: 1s ease-in 1s 1 slideAnimation;
    animation-fill-mode: forwards;
}

.podium1e {
    height: 130px;
    transform: translateY(-100vh);
    animation: 1s ease-in 2s 1 slideAnimation;
    animation-fill-mode: forwards;
}

.playerName {
    font-weight: 600;
    font-size: 20px;
}

.playerScore {
    font-weight: 500;
    font-size: 16px;
}

.playerName,
.playerScore,
.place {
    display: block;
    text-align: center;
}

.playerScoreContainer {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 55px;
    top: -60px;
}

.place.one {
    font-size: 32px;
    color: #ffd700;
    transform: translateY(-50%);
}

.place.two {
    font-size: 28px;
    color: #C0C0C0;
    transform: translateY(-75%);
}

.place.three {
    font-size: 24px;
    color: #cd7f32;
    transform: translateY(-100%);
}

.place {
    font-weight: 600;
    vertical-align: top;
}

.restContainer {
    font-size: 16px;
    width: 100%;
    border-collapse: collapse;
    border: 2px solid #99AA38;
    opacity: 0;
    animation: 1s ease-out 4s 1 fadeInAnimation;
    animation-fill-mode: forwards;
}

.restContainer>tr,
.restContainer>tr>td {
    border: 2px solid #99AA38;
}

.positionRest {
    width: 50px;
    text-align: center;
    font-weight: 600;
}

.playerNameRest,
.playerScoreRest {
    padding-left: 10px;
}

.newGameContainer {
    display: flex;
    justify-content: center;
    margin-bottom: 50px;
}

.newGame {
    border-radius: 25px;
    border: none;
    padding: 5px 10px;
    font-weight: 600;
    font-size: 20px;
    background-color: #0A210F;
    color: white;
}

.underPodiumContainer {
    display: flex;
    flex-grow: 1;
    flex-direction: column;
    justify-content: space-between;
}

@keyframes slideAnimation {
    0% {
        transform: translateY(-100vh);
    }

    100% {
        transform: translateY(0);
    }
}

@keyframes fadeInAnimation {
    0% {
        opacity: 0;
    }

    100% {
        opacity: 1;
    }
}
</style>