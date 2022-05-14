<template>
    <div class="screen">
        <h1>PIKACHU GAME</h1>
        <p>Select mode to start game</p>
        <div class="modes">
            <button @click="onStart(16)">
                <span>8x2</span>
                <span>Easy</span>
            </button>
            <button @click="onStart(36)">
                <span>9x4</span>
                <span>Medium</span>
            </button>
            <button @click="onStart(48)">
                <span>12x4</span>
                <span>Hard</span>
            </button>
            <button @click="onStart(80)">
                <span>16x5</span>
                <span>Insane</span>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    methods: {
        onStart(numCards) {
            this.$emit('onStart', { numCards: numCards });
        }
    }
};
</script>

<style scoped>
.screen {
    width: 100%;
    height: 100vh;
    margin: 0;
    display: flex;
    flex-direction: column;
    padding-top: 10%;
    align-items: center;
    background-color: var(--dark);
    color: var(--light);
}

h1 {
    font-size: 4.5rem;
    text-transform: uppercase;
}

p {
    font-size: 2rem;
}

.modes {
    margin-top: 2rem;
}

.modes button {
    width: 150px;
    height: 150px;
}

.modes button span {
    display: flex;
    align-items: center;
    justify-content: center;
}

.modes button span:first-child {
    font-size: 2rem;
}

.modes button span:last-child {
    font-size: 1.25rem;
    margin-top: 0.5rem;
}
</style>
