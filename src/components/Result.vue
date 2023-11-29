<template>
    <div class="tip-calculator-result">
        <div class="tip-calculator-result-title">
            <p>{{ title }}</p>
            <p>/ person</p>
        </div>
        <p class="tip-calculator-result-number">${{ computedResult }}</p>
    </div>
</template>

<script>
    export default {
        name: 'Result',
        props: ['title', 'tipPercentage', 'billAmount', 'peopleNum'],
        data() {
            return {
                result: 0,
            }
        },
        computed: {
            computedResult: function() {
                if (this.peopleNum === '') {
                    switch (this.title) {
                        case 'Tip Amount':
                            return (this.billAmount * this.tipPercentage / 1).toFixed(2)
                            break
                        case 'Total':
                            return (this.billAmount * (1 + this.tipPercentage) / 1).toFixed(2)
                    }
                } else {
                    switch (this.title) {
                        case 'Tip Amount':
                            return (this.billAmount * this.tipPercentage / this.peopleNum).toFixed(2)
                            break
                        case 'Total':
                            return (this.billAmount * (1 + this.tipPercentage) / this.peopleNum).toFixed(2)
                    }
                }
            }
        }
    }
</script>

<style lang="scss">
    @import '../scss/variables';

    .tip-calculator-result {
        display: flex;
        align-items: center;

        &:first-child {
            margin-top: $lg;
        }

        &:nth-child(2) {
            margin-bottom: 75px;
        }

        &-title {
            flex-grow: 1;

            p {
                &:first-child {
                    color: $very-light-gray-cyan;
                    font-size: .6em;
                }
    
                &:last-child {
                    color: $gray-cyan;
                    font-size: .5em;
                }
            }
        }

        &-number {
            font-size: 1.5em;
            color: $cyan;
        }
    }

    @media screen and (max-width: 768px) {
        .tip-calculator-result {
            &:nth-child(2) {
                margin-bottom: 0px;
            }
        }
    }
</style>