<template>
    <div id="app">
        <main-screen
            v-if="statusGame === 'beforeGame'"
            @onStart="onHandleStart($event)"
        />
        <interact-screen
            v-if="statusGame === 'inGame'"
            :cardsArr="settings.cardsArrDouble"
            @onFinish="onGetResult()"
            @backToMainScreen="statusGame = 'beforeGame'"
        />
        <result-screen
            v-if="statusGame === 'completeGame'"
            :timer="timer"
            @onStartAgain="statusGame = 'beforeGame'"
        />
        <the-footer />
    </div>
</template>

<script>
import MainScreen from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen.vue';
import ResultScreen from './components/ResultScreen.vue';
import TheFooter from './components/TheFooter.vue';

import { shuffleArr } from './utils/array';
export default {
    name: 'App',
    components: {
        MainScreen,
        InteractScreen,
        ResultScreen,
        TheFooter
    },
    data() {
        return {
            settings: {
                numCards: 0,
                cardsArrDouble: [],
                startedAt: null
            },
            statusGame: 'beforeGame',
            timer: 0
        };
    },
    methods: {
        onHandleStart(config) {
            this.settings.numCards = config.numCards;

            const cardsArrSingle = Array.from(
                { length: this.settings.numCards / 2 },
                (_, i) => i + 1
            );
            const cards = [...cardsArrSingle, ...cardsArrSingle];

            this.settings.cardsArrDouble = shuffleArr(shuffleArr(cards));

            this.settings.startedAt = new Date().getTime();

            this.statusGame = 'inGame';
        },
        onGetResult() {
            this.timer = new Date().getTime() - this.settings.startedAt;

            this.statusGame = 'completeGame';
        }
    }
};
</script>

<style>
#app {
    width: 100%;
    height: 100vh;
    margin: 0;
    box-sizing: border-box;
    position: relative;
}
</style>
