<template>
    <div class="container">
        <span class="name">{{ props.playerName }}</span>
        <button class="deletePlayer" @click="$emit('deletePlayer')">
            <div class="minusSymbol"></div>
        </button>
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
    border: 2px white solid;
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    width: 75%;
    color: black;
    font-size: 16px;
}
.deletePlayer {
    background-color: transparent;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    border: none;
    display: flex;
    justify-content: center;
    align-items: center;
}

.minusSymbol{
    background-color: rgb(211, 32, 32);
    height: 4px;
    width: 14px;
}
</style>