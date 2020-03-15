<template>
    <div id="root">
        <transition name="fade" mode="out-in" appear>
            <div id="grid" v-if="isSelecting || isShowing" :key="state">
                <card @click="selectCard('0')" :disabled="isShowing && !isSelectedCard('0')">0</card>
                <card @click="selectCard('1')" :disabled="isShowing && !isSelectedCard('1')">1</card>
                <card @click="selectCard('2')" :disabled="isShowing && !isSelectedCard('2')">2</card>
                <card @click="selectCard('3')" :disabled="isShowing && !isSelectedCard('3')">3</card>
                <card @click="selectCard('5')" :disabled="isShowing && !isSelectedCard('5')">5</card>
                <card @click="selectCard('8')" :disabled="isShowing && !isSelectedCard('8')">8</card>
                <card @click="selectCard('13')" :disabled="isShowing && !isSelectedCard('13')">13</card>
                <card @click="selectCard('21')" :disabled="isShowing && !isSelectedCard('21')">21</card>
                <card @click="selectCard('34')" :disabled="isShowing && !isSelectedCard('34')">34</card>
                <card @click="selectCard('55')" :disabled="isShowing && !isSelectedCard('55')">55</card>
                <card @click="selectCard('89')" :disabled="isShowing && !isSelectedCard('89')">89</card>
                <card @click="selectCard('144')" :disabled="isShowing && !isSelectedCard('144')">144</card>
                <card @click="selectCard('∞')" :disabled="isShowing && !isSelectedCard('∞')">∞</card>
                <card @click="selectCard('?')" :disabled="isShowing && !isSelectedCard('?')">?</card>
                <card @click="selectCard('c')" :disabled="isShowing && !isSelectedCard('c')">c</card>
            </div>
            <div id="actions" v-if="isWaiting" key="isWaiting">
                <button id="edit" @click="edit">edit</button>
                <button id="show" @click="show">show</button>
            </div>
        </transition>
    </div>
</template>

<script>
import Card from "./components/Card.vue";

export default {
    components: {
        Card
    },
    data() {
        return {
            state: "selection",
            card: 0
        };
    },
    methods: {
        vibrate(duration) {
            if (!window) {
                return;
            }

            if (!window.navigator) {
                return;
            }

            if (!window.navigator.vibrate) {
                return;
            }

            if (arguments.length !== 1) {
                throw new Error("Expected exactly one argument.");
            }

            if (typeof duration !== "number") {
                throw new TypeError("Expected first argument to be a number.");
            }

            if (!Number.isInteger(duration)) {
                throw new TypeError("Expected first argument to be an integer.");
            }

            window.navigator.vibrate(duration);
        },
        selectCard(card) {
            if (this.isShowing) {
                this.state = "selection";
                this.card = null;
                return;
            }

            this.card = card;
            this.state = "waiting";

            this.vibrate(100);
        },
        edit() {
            this.state = "selection";

            this.vibrate(100);
        },
        show() {
            this.state = "show";

            this.vibrate(100);
        },
        isSelectedCard(card) {
            return this.card === card;
        }
    },
    computed: {
        isSelecting() {
            return this.state === "selection";
        },
        isWaiting() {
            return this.state === "waiting";
        },
        isShowing() {
            return this.state === "show";
        }
    }
};
</script>

<style scoped>

#root {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    width: 100%;
}

#grid {
    --grid-gap: 10px;
    padding: var(--grid-gap);
    display: grid;
    grid-template-columns: repeat(3, calc(33vw - (var(--grid-gap) * 2)));
    grid-auto-rows: calc(100vh / 5 - (var(--grid-gap) * 2));
    justify-items: center;
    align-items: center;
    grid-gap: var(--grid-gap);
}

@media(orientation: landscape) {
    #grid {
        grid-template-columns: repeat(5, calc(33vh - (var(--grid-gap) * 2)));
        grid-auto-rows: calc(100vh / 3 - (var(--grid-gap) * 2));
    }
}

#edit {
    border: 1px solid darkblue;
    background-color: transparent;
    color: darkblue;
    text-transform: uppercase;
    font-size: 2rem;
    padding: 10px 20px;
    border-radius: 10px;
}

#show {
    border: none;
    background-color: darkblue;
    color: white;
    text-transform: uppercase;
    font-size: 2rem;
    padding: 10px 20px;
    border-radius: 10px;
    margin-left: 10px;
}

.fade-enter {
    opacity: 0;
}

.fade-enter-to {
    opacity: 1;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.25s ease-in-out;
}

.fade-leave {
    opacity: 1;
}

.fade-leave-to {
    opacity: 0;
}
</style>

<style>
html, body {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
}
</style>
