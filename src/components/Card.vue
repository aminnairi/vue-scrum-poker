<template>
    <div
        class="card"
        v-on="$listeners"
        v-bind="$attrs"
        :class="classes">
        <slot>
        </slot>
    </div>
</template>

<script>
export default {
    props: {
        disabled: {
            type: Boolean,
            default: false
        }
    },
    computed: {
        classes() {
            return {
                disabled: this.disabled
            };
        }
    }
};
</script>

<style scoped>
.card {
    height: 100%;
    width: 100%;
    background-color: darkblue;
    border: 1px solid white;
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    font-size: 13vmin;
    font-family: sans-serif;
    text-shadow: 1px 1px 10px white;
}

.disabled {
    background-color: transparent;
    color: transparent;
}
</style>
