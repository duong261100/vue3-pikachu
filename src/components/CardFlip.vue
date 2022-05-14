<template>
    <div
        class="card"
        :class="{ disabled: isDisabled }"
        :style="{
            width: `${this.cardWidth}px`,
            height: `${(this.cardWidth / 3) * 4}px`
        }"
    >
        <div
            class="card__inner"
            :class="{ 'is-flipped': isFlipped }"
            @click="onToggleFlipCard()"
        >
            <div
                class="card__face card__face--front"
                :style="{
                    padding: `${this.padding}rem`
                }"
            >
                <div class="card__content" />
            </div>
            <div class="card__face card__face--back">
                <div
                    class="card__content"
                    :style="{
                        backgroundImage: `url(${require('@/assets/' +
                            imgBackFaceUrl)})`
                    }"
                />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        imgBackFaceUrl: {
            type: String,
            required: true
        },
        stt: {
            type: [String, Number, Array, Object],
            required: true
        },
        cardWidth: {
            type: Number,
            required: true
        },
        padding: {
            type: Number,
            required: true
        }
    },
    data() {
        return {
            isFlipped: false,
            isDisabled: false
        };
    },
    methods: {
        onToggleFlipCard() {
            if (this.isDisabled) return false;

            this.isFlipped = !this.isFlipped;
            if (this.isFlipped) this.$emit('onFlip', this.stt);
            else this.$emit('onFaceDown');
        },
        onFlipBackCard() {
            this.isFlipped = false;
        },
        onDisable() {
            this.isDisabled = true;
        }
    }
};
</script>

<style lang="css" scoped>
.card {
    display: inline-block;
    margin: 1rem 1rem;
}

.card.disabled .card__inner {
    cursor: default;
}

.card__inner {
    width: 100%;
    height: 100%;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 1s;
    cursor: pointer;
}

.card__inner.is-flipped {
    transform: rotateY(-180deg);
}

.card__face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0px 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
    background: url('../assets/images/icon_back.png') no-repeat center center;
    background-size: 40px 40px;
    height: 100%;
    width: 100%;
}

.card__face--back {
    background-color: var(--light);
    transform: rotateY(-180deg);
}

.card__face--back .card__content {
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
}
</style>
