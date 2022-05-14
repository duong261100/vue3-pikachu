<template>
    <div class="screen">
        <h1>PIKACHU GAME</h1>
        <div
            class="screen__inner"
            :style="{
                width: `${matrixStyle.matrixWidth}px`,
                height: `${matrixStyle.matrixHeight}vh`
            }"
        >
            <card-flip
                v-for="(card, index) in cardsArr"
                :key="index"
                :stt="index"
                ref="cards"
                :imgBackFaceUrl="`images/${card}.png`"
                @onFlip="checkRule($event)"
                @onFaceDown="resetCurrentSelect()"
                :cardWidth="matrixStyle.cardWidth"
                :padding="matrixStyle.padding"
            />
        </div>
        <button @click="backToMainScreen">Back</button>
    </div>
</template>

<script>
import CardFlip from './CardFlip.vue';

export default {
    props: {
        cardsArr: {
            type: Array,
            default: function () {
                return [];
            }
        }
    },
    components: {
        CardFlip
    },
    data() {
        return {
            currentIndexSelect: [],
            matrixStyle: {
                matrixWidth: 0,
                matrixHeight: 60,
                cardWidth: 0,
                padding: 1
            }
        };
    },
    methods: {
        checkRule(index) {
            if (this.currentIndexSelect.length === 2) return false;

            this.currentIndexSelect.push(index);

            const firstImageSelect = this.cardsArr[this.currentIndexSelect[0]];
            const secondImageSelect = this.cardsArr[this.currentIndexSelect[1]];

            if (this.currentIndexSelect.length === 2) {
                if (
                    firstImageSelect === secondImageSelect &&
                    this.currentIndexSelect[0] != this.currentIndexSelect[1]
                ) {
                    this.$refs.cards[this.currentIndexSelect[0]].onDisable();
                    this.$refs.cards[this.currentIndexSelect[1]].onDisable();

                    this.resetCurrentSelect();

                    const numCompletedCards =
                        document.querySelectorAll('.card.disabled');
                    console.log(numCompletedCards.length);
                    console.log(this.cardsArr.length);
                    if (
                        numCompletedCards &&
                        numCompletedCards.length === this.cardsArr.length - 2
                    ) {
                        setTimeout(() => {
                            this.$emit('onFinish');
                        }, 500);
                    }
                } else {
                    setTimeout(() => {
                        this.$refs.cards[
                            this.currentIndexSelect[0]
                        ].onFlipBackCard();
                        this.$refs.cards[
                            this.currentIndexSelect[1]
                        ].onFlipBackCard();
                        this.resetCurrentSelect();
                    }, 800);
                }
            }
        },
        resetCurrentSelect() {
            this.currentIndexSelect = [];
        },
        calculateMatrixStyle() {
            const numsCard = this.cardsArr.length;

            if (numsCard === 16) {
                this.matrixStyle.matrixWidth = 976;
                this.matrixStyle.cardWidth = 90;
                this.matrixStyle.padding = 1;
            } else if (numsCard === 36) {
                this.matrixStyle.matrixWidth = 900;
                this.matrixStyle.cardWidth = 68;
                this.matrixStyle.padding = 1;
            } else if (numsCard === 48) {
                this.matrixStyle.matrixWidth = 1200;
                this.matrixStyle.cardWidth = 68;
                this.matrixStyle.padding = 1;
            } else if (numsCard === 80) {
                this.matrixStyle.matrixWidth = 1280;
                this.matrixStyle.cardWidth = 48;
                this.matrixStyle.padding = 0;
            }
        },
        backToMainScreen() {
            this.$emit('backToMainScreen');
        }
    },
    beforeMount() {
        this.calculateMatrixStyle();
    }
};
</script>

<style lang="css" scoped>
.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    background-color: var(--dark);
    color: var(--light);
}

h1 {
    font-size: 3rem;
    text-transform: uppercase;
    text-align: center;
    margin-top: 40px;
}

.screen__inner {
    display: flex;
    flex-wrap: wrap;
    margin: 1.75rem auto;
}

button {
    margin: 3rem 10rem 0 0;
    float: right;
    padding: 0.75rem 2.5rem;
    border-radius: 0.5rem;
    font-size: 1.25rem;
}
</style>
