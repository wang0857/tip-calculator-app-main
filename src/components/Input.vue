<template>
    <div>
        <div v-if="!isCustomBtn" class="tip-calculator-input-title-container">
            <label :for="title" class="tip-calculator-input-title">{{ title }}</label>
            <span v-if="error !== ''" class="tip-calculator-input-errorMsg">{{ error }}</span>
        </div>
        <div class="tip-calculator-input">
            <img v-if="icon" :src="icon" alt="icon">
            <input
                type="number"
                v-model="value"
                :placeholder="placeholder"
                :id="title"
                :class="(isCustomBtn && 'custom') || (error !== '' && 'error')"
            >
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Input',
        props: ['title', 'placeholder', 'icon', 'isCustomBtn'],
        emits: ['disableReset', 'setTipPercentage', 'setBillAmount', 'setPeopleNum'],
        data() {
            return {
                value: '',
                error: ''
            }
        },
        updated() {
            const inputs = document.querySelectorAll('.tip-calculator-input')
            inputs.forEach(input => {
                if (input.value !== 0) {
                    this.$emit('disableReset', false)
                }
            })
        },
        watch: {
            value: function() {
                // TODO: Fixed the disable reset button when one input has value and the other doesn't has value
                if (this.value === 0 || this.value === '') {
                    this.$emit('disableReset', true)
                } else {
                    this.$emit('disableReset', false)
                }

                if (this.isCustomBtn) {
                    // Reset the class of existing active tip buttons 
                    const tipBtns = document.querySelectorAll('.tip')
                    tipBtns.forEach(btn => {
                        btn.classList.remove('active')
                    })

                    this.$emit('setTipPercentage', this.value / 100)
                } else {
                    if (this.value === 0) {
                        this.error = "Can't be zero"
                    } else {
                        this.error = ''
                        switch (this.title) {
                            case 'Bill':
                                this.$emit('setBillAmount', this.value)
                                break
                            case 'Number of People':
                                this.$emit('setPeopleNum', this.value)
                                break
                        }
                    }
                }
            }
        }
    }
</script>

<style lang="scss">
    @import '../scss/variables';

    .tip-calculator-input {
        position: relative;

        img {
            position: absolute;
            left: $lg;
            top: $md;
        }

        input {
            background-color: $very-light-gray-cyan;
            border-radius: $xs;
            height: 45px;
            width: calc(100% - 40px);
            border: none;
            text-align: end;
            padding: 0px $lg;
            font-family: 'Space Mono', monospace;
            font-size: .7em;
            color: $dark-cyan;

            &::placeholder {
                color: $gray-cyan;
                opacity: 1;
            }

            &:focus, &:focus-visible {
                outline: 2px solid $cyan;
            }

            /* Hide spinner */
            /* Chrome */
            &::-webkit-outer-spin-button,
            &::-webkit-inner-spin-button {
                -webkit-appearance: none;
                margin: 0;
            }

            /* Firefox */
            &[type=number] {
                -moz-appearance: textfield;
            }

            &.custom {
                font-size: .75em;
                color: $dark-cyan;
                padding: 15px;
                height: 100%;
                width: calc(100% - 30px);
                text-align: center;
                border-radius: $sm;

                &::placeholder {
                    color: $dark-gray-cyan;
                    opacity: 1;
                }
            }

            &.error {
                outline: 2px solid #D47D62;
            }
        }

        &-title {
            font-size: .5em;
            margin-bottom: $sm;

            &-container {
                display: flex;
                justify-content: space-between;
            }
        }

        &-errorMsg {
            color: #D47D62;
            font-size: .5em;
        }
    }

    @media screen and (max-width: 768px) {
        .tip-calculator-input {
            input {
                &.custom {
                    text-align: end;
                }
            }
        }
    }
</style>