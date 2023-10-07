<template>
    <div class="container">
        <span class="name">{{ props.playerName }}</span>
        <button class="deletePlayer" @click="$emit('deletePlayer')">-</button>
    </div>
</template>

<script setup>
const props = defineProps({
    playerName: {
        type: String,
        default: "Username"
    }
})
</script>

<style scoped>
.container {
    background-color: white;
    border-radius: 25px;
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    width: 75%;
    color: black;
    font-size: 16px;
}
.deletePlayer {
    color: white;
    background-color: black;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    font-size: 30px;
    text-align: center;
    line-height: 0;
    padding: 0px 0px 5px 0px;
    border: none;
}
</style>