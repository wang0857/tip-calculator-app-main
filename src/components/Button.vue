<template>
    <button
        class="tip-calculator-btn"
        :disabled="disableReset"
        :class="tip ? 'tip' : disableReset ? '' : 'active'"
        @click="setTip"
    >
        <slot></slot>
    </button>
</template>

<script>
    export default {
        name: 'Button',
        props: ['disableReset', 'tip'],
        emits: ['setTipPercentage'],
        methods: {
            setTip(e) {
                // Prevent the page from refreshing
                e.preventDefault()

                if (e.target.classList.contains('active')) {
                    // Toggle class when clicking on the active tip button
                    e.target.classList.toggle('active')
                } else {
                    // Reset the class of existing active tip buttons 
                    const tipBtns = document.querySelectorAll('.tip')
                    tipBtns.forEach(btn => {
                        btn.classList.remove('active')
                    })
    
                    // Set the clicked tip button to be active
                    e.target.classList.add('active')
                }

                this.$emit('setTipPercentage', Number(this.tip))
            },
        },
        watch: {
            reset: function() {
                const tipBtns = document.querySelectorAll('.tip')
                tipBtns.forEach(btn => {
                    btn.classList.remove('active')
                })
            }
        }
    }
</script>

<style lang="scss">
    @import '../scss/variables';

    .tip-calculator-btn {
        border: none;
        background-color: $dark-cyan;
        color: $very-light-gray-cyan;
        font-size: .75em;
        font-family: 'Space Mono', monospace;
        border-radius: $sm;
        transition: $transition;
        padding: $md;
        text-transform: uppercase;

        &:hover, &:not(:disabled):hover {
            cursor: pointer;
            background-color: #9FE8DF;
            color: $dark-cyan;
        }

        &.active {
            background-color: $cyan;
            color: $dark-cyan;
        }

        &:disabled {
            background-color: #0D686D;
            color: $dark-cyan;
            cursor: initial;
        }
    }
</style>