<template>
  <header>
    <h2>SPLI</h2>
    <h2>TTER</h2>
  </header>
  <div class="tip-calculator-container">
    <form class="tip-calculator-input-form">
      <Input
        title="Bill"
        placeholder="0"
        :icon="dollarIcon"
        @disableReset="resetStart"
        @setBillAmount="setBillAmount"
      />
      <div>
        <h3 class="tip-calculator-title">Select Tip %</h3>
        <div class="tip-calculator-percent">
          <Button tip="0.05" @setTipPercentage="setTipPercentage">5%</Button>
          <Button tip="0.10" @setTipPercentage="setTipPercentage">10%</Button>
          <Button tip="0.15" @setTipPercentage="setTipPercentage">15%</Button>
          <Button tip="0.25" @setTipPercentage="setTipPercentage">25%</Button>
          <Button tip="0.50" @setTipPercentage="setTipPercentage">50%</Button>
          <Input
            placeholder="Custom"
            :isCustomBtn="true"
            @setTipPercentage="setTipPercentage"
          />
        </div>
      </div>
      <Input
        title="Number of People"
        placeholder="0"
        :icon="personIcon"
        @disableReset="resetStart"
        @setPeopleNum="setPeopleNum"
      />
    </form>
    <div class="tip-calculator-calculator">
      <Result
        title="Tip Amount"
        :tipPercentage="tipPercentage"
        :billAmount="billAmount"
        :peopleNum="peopleNum"
      />
      <Result
        title="Total"
        :tipPercentage="tipPercentage"
        :billAmount="billAmount"
        :peopleNum="peopleNum"
      />
      <Button
        :disableReset="disableReset"
        @click="resetTipCalculator"
      >Reset</Button>
    </div>
  </div>
</template>

<script>
  import Input from './components/Input.vue';
  import Button from './components/Button.vue';
  import Result from './components/Result.vue';

  import dollarIcon from '../src/assets/icon-dollar.svg'
  import personIcon from '../src/assets/icon-person.svg'

  export default {
    name: 'App',
    components: { Input, Button, Result },
    data() {
      return {
        dollarIcon: dollarIcon,
        personIcon: personIcon,
        disableReset: true,
        tipPercentage: 0,
        billAmount: 0,
        peopleNum: 1
      }
    },
    methods: {
      resetStart(reset) {
        this.disableReset = reset
      },
      setTipPercentage(percentage) {
        this.tipPercentage = percentage
      },
      setBillAmount(amount) {
        this.billAmount = amount
      },
      setPeopleNum(num) {
        this.peopleNum = num
      },
      resetTipCalculator() {
        // Reset form
        const form = document.querySelector('.tip-calculator-input-form')
        form.reset()

        // Reset the class of existing active tip buttons 
        const tipBtns = document.querySelectorAll('.tip')
          tipBtns.forEach(btn => {
            btn.classList.remove('active')
        })

        // Reset the result calculation
        this.tipPercentage = 0
        this.billAmount = 0
        this.peopleNum = 1
        this.disableReset = true
      }
    },
  }
</script>

<style lang="scss">

  @import './scss/variables';

  header {
    margin-bottom: 88px;

    h2 {
      font-size: .75em;
      text-align: center;
      letter-spacing: 14px;
    }
  }

  .tip-calculator {
    &-container {
      width: 920px;
      border-radius: 25px;
      background-color: $white;
      padding: 50px;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 46px;
    }

    &-input-form {
      display: flex;
      flex-direction: column;
      gap: 50px;
    }

    &-title {
      font-size: .5em;
      margin-bottom: $lg;
    }

    &-percent {
      display: grid;
      gap: $md;
      grid-template-columns: repeat(3, 1fr);
    }

    &-calculator {
      border-radius: $md;
      background-color: $dark-cyan;
      padding: 40px;
      display: flex;
      flex-direction: column;
      gap: 60px;
    }
  }

  @media screen and (max-width: 768px) {
    header {
      margin: 50px 0px 40px;
    }

    .tip-calculator {
      &-container {
        width: unset;
        min-width: calc(100vw - 60px);
        grid-template-columns: 1fr;
        padding: 40px 30px;
        border-bottom-left-radius: 0px;
        border-bottom-right-radius: 0px;
        gap: 30px;
      }

      &-input-form {
        gap: 40px;
      }

      &-percent {
        grid-template-columns: 1fr 1fr;
      }

      &-calculator {
        padding: 25px;
        gap: $xlg;
      }
    }
  }
</style>
