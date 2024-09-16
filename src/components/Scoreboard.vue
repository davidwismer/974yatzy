<template>
    <div class="yatzyTableContainer">
        <table id="scoreboard-table">
            <tr id="players">
                <th class="label">Yatzeurs</th>
                <th v-for="(name) of props.playerNames">{{ name }}</th>
            </tr>
            <tr v-for="(row) of ROW_INPUT_NUMBER" :id="row.id">
                <td class="label" :class="row.id">{{ row.label }}</td>
                <td v-for="(player) of props.players" class="number" :class="[row.id, player.name]">
                    <input type="number" :class="[row.id, player.name]" @change="updateScore(player, row)">
                </td>
            </tr>
        </table>
    </div>
</template>

<script setup>

const props = defineProps({
    playerNames: {
        type: Array,
        required: true
    },
    players: {
        type: Array,
        required: true
    }
})

const emits = defineEmits(['updateTotalScore'])

const ROW_INPUT_NUMBER = {
    ones: {
        label: "Un",
        id: "ones",
        category: "numbers"
    },
    twos: {
        label: "Deux",
        id: "twos",
        category: "numbers"
    },
    threes: {
        label: "Trois",
        id: "threes",
        category: "numbers"
    },
    fours: {
        label: "Quatre",
        id: "fours",
        category: "numbers"
    },
    fives: {
        label: "Cinq",
        id: "fives",
        category: "numbers"
    },
    six: {
        label: "Six",
        id: "six",
        category: "numbers"
    },
    totalBonus: {
        label: "Total",
        id: "total-bonus",
        category: "numbers"
    },
    bonus: {
        label: "Bonus",
        id: "bonus",
        category: "numbers"
    },
    pair: {
        label: "Paire",
        id: "pair",
        category: "combination"
    },
    doublePair: {
        label: "2 Paires",
        id: "double-pair",
        category: "combination"
    },
    threeSame: {
        label: "Brelan",
        id: "three-same",
        category: "combination"
    },
    fourSame: {
        label: "Carré",
        id: "four-same",
        category: "combination"
    },
    smallStraight: {
        label: "Petite suite",
        id: "small-straight",
        category: "combination"
    },
    bigStraight: {
        label: "Grande suite",
        id: "big-straight",
        category: "combination"
    },
    full: {
        label: "Full",
        id: "full",
        category: "combination"
    },
    chance: {
        label: "Chance",
        id: "chance",
        category: "combination"
    },
    yatzy: {
        label: "Yatzy",
        id: "yatzy",
        category: "combination"
    }
}

function updateScore(player, row) {
    const totalBonusCell = document.querySelector(`input.total-bonus.${player.name}`)
    // Change total for bonus if in lower numbers
    const arr = ['ones', 'twos', 'threes', 'fours', 'fives', 'six']
    if (row.category == "numbers") {
        let topTotal = 0;
        let topComplete = 0;
        arr.forEach(row => {
            const cellValue = parseInt(document.querySelector(`input.${row}.${player.name}`).value)
            if (!isNaN(cellValue)) {
                topTotal += cellValue
                topComplete++
            }
        })
        //Set topTotal on scoreboard
        totalBonusCell.value = topTotal
        if(topTotal == 0) totalBonusCell.value = ''
        //Set the bonus if every top cell is complete
        if (topComplete == 6) {
            if(topTotal >= 63){
                document.querySelector(`input.bonus.${player.name}`).value = 25
            }else {
                document.querySelector(`input.bonus.${player.name}`).value = 0
            }
        }
    }
    // Change total score at the end of game
    const allInputs = document.querySelectorAll(`input.${player.name}`)
    let filled = true
    let totalScore = 0
    allInputs.forEach(input => {
        if(isNaN(parseInt(input.value))){
            filled = false
        }else if(!(input.classList[0] == "total-bonus")) {
            totalScore += parseInt(input.value)
        }
    })
    if(filled){
        emits("updateTotalScore", totalScore, player)
    }
}
</script>

<style scoped>
table {
    height: 100%;
    border-collapse: collapse;
    border: 2px solid #99AA38;
    color: black;
}

table > :nth-child(2n){
    background-color: #d8da71;
}

td {
    border: 1px solid #99AA38;
    border-right: 2px solid #99AA38;
    padding: 0;
}

td:not(.label) {
    text-align: center;
}

th {
    border: 2px solid #99AA38;
    font-weight: 600;
}

th:not(.label) {
    max-width: 35px;
    min-width: 35px;
    width: 35px;
}

.label {
    height: 30px;
    white-space: nowrap;
    padding-left: 10px;
    padding-right: 5px;
    border-right: 2px solid #99AA38;
}

#six,
#yatzy,
#bonus {
    border-bottom: 2px solid #99AA38;
}

.total-bonus {
    font-weight: 600;
    font-size: 16px;
}

input {
    width: 100%;
    height: 100%;
    border: none;
    background-color: transparent;
    text-align: center;
}

input[type=number]:focus {
    outline: none;
}

input.total-bonus,
input.bonus {
    pointer-events: none;
}
</style>