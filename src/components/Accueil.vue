<template>
    <div class="accueilContainer">
        <h1>Yatzy scoreboard</h1>
        <div class="playerSection">
            <div class="userField">
                <input class="userField-input" type="text" placeholder="Ajouter un joueur">
                <button class="userField-button" @click="addPlayer">+</button>
            </div>
            <div class="playerContainer">
                <player-name v-for="player of players" :playerName="player.name" @deletePlayer="deletePlayer(player)">
                </player-name>
            </div>
        </div>
        <div class="playBtnContainer">
            <button class="playBtn" :class="{disabled: notEnoughPlayer}" @click="play()">Jouer</button>
            <span class="warning" :class="{disabled: notEnoughPlayer}">Pas de joueurs</span>
        </div>
    </div>
</template>

<script setup>
import PlayerName from './PlayerName.vue';
import { ref } from 'vue';

const players = ref([])
const notEnoughPlayer = ref(false)
const emits = defineEmits(["play"])

function addPlayer() {
    const name = document.querySelector(".userField-input")
    if (name.value) {
        players.value.push({
            name : name.value.charAt(0).toUpperCase() + name.value.slice(1),
            totalScore: 0
        })
        name.value = ""
    }
    notEnoughPlayer.value = false
    document.querySelector(".userField-input").focus()
}

function deletePlayer(name) {
    if (name) {
        players.value.splice(players.value.findIndex((e) => e == name), 1)
    }
}

function play(){
    if(players.value.length < 1){
        notEnoughPlayer.value = true
    }else {
        emits("play", players.value)
    }
}
</script>

<style scoped>
h1 {
    color: #0A210F;
}

.accueilContainer {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
}

.playerSection {
    margin-top: 30px;
    width: 100%;
    height: 50%;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.userField {
    background-color: #E1E289;
    border: 2px #0A210F solid;
    border-radius: 25px;
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    width: 75%;
}

.userField-button {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    font-size: 30px;
    text-align: center;
    line-height: 0px;
    padding: 0px;
    border: none;
    background-color: black;
    color: #E1E289;
}

.userField-input {
    border: none;
    background-color: #E1E289;
    color: black;
    font-size: 16px;
    width: 80%;
}

.userField-input::placeholder {
    color: black;
    font-size: 16px;
    opacity: 0.8;
}

input[type=text]:focus {
    outline: none;
}

.playerContainer {
    overflow-y: auto;
    margin-top: 10px;
    margin-bottom: 10px;
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 10px;
    align-items: center;
}
.playBtnContainer {
    width: 100%;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #E1E289;
}

.playBtn {
    margin-top: 20px;
    width: 50%;
    border-radius: 25px;
    border: none;
    padding: 5px 10px;
    font-weight: 800;
    font-size: 28px;
    background-color: #0A210F;
    color: white;
}
button.disabled {
    opacity: 0.4;
}
.warning {
    color: red;
    display: none;
}
span.disabled {
    display: block;
}
</style>