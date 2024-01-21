<template>
    <div class="calculator">
        <div class="calculator__header">
            <div v-if="operation" class="calculator__small">
              {{ operation }}
            </div>
            <input type="text" @keyup.enter="calculate()" class="calculator__input" :placeholder="result" v-model="expression" />
        </div>
        <div class="calculator__buttons">
            <span class="calculator__buttons__item" 
                @click="startCommand(button.value, button.type)"
                v-for="(button, index) in buttons"
                :class="button.class"
                :style="{order: button.order}"
                :key="index">
                {{ button.name }}
            </span>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
const operation = ref('');
const expression = ref('');
const result = ref(0);

const buttons = [{ order: 17, name: '0', value: '0', type: 'operation', class: ''},
      { order: 4, name: '1', value: '1', type: 'operation', class: ''},
      { order: 5, name: '2', value: '2', type: 'operation', class: ''},
      { order: 6, name: '3', value: '3', type: 'operation', class: ''},
      { order: 8, name: '4', value: '4', type: 'operation', class: ''},
      { order: 9, name: '5', value: '5', type: 'operation', class: ''},
      { order: 10, name: '6', value: '6', type: 'operation', class: ''},
      { order: 12, name: '7', value: '7', type: 'operation', class: ''},
      { order: 13, name: '8', value: '8', type: 'operation', class: ''},
      { order: 14, name: '9', value: '9', type: 'operation', class: ''},
      { order: 3, name: '+', value: '+', type: 'operation', class: 'operation'},
      { order: 7, name: '—', value: '-', type: 'operation', class: 'operation'},
      { order: 11, name: '×', value: '*', type: 'operation', class: 'operation'},
      { order: 15, name: '/', value: '/', type: 'operation', class: 'operation'},
      { order: 1, name: '(', value: '(', type: 'operation', class: 'operation'},
      { order: 2, name: ')', value: ')', type: 'operation', class: 'operation'},
      { order: 18, name: '%', value: '%', type: 'percentage', class: 'operation'},
      { order: 19, name: '=', value: '=', type: 'calculate', class: 'operation result'},
      { order: 16, name: '.', value: '.', type: 'operation', class: 'operation'},
      { order: 0, name: 'C', value: 'C', type: 'reset', class: 'operation cancel'}];

      const startCommand = (buttonValue, buttonType) => {

        switch(buttonType) {
          case 'calculate':
            calculate();
            break;
          
          case 'reset': 
            resetValue();
            break;

          case 'percentage': 
            makePercentage();
            break;
          
          default:
            expression.value += buttonValue;
        }
        
      }

      const calculate = () => {
        operation.value = expression.value;
        result.value = eval(expression.value);
        expression.value = '';
      }

      const resetValue = () => {
        expression.value = '';
        operation.value = '';
        result.value = 0;
      }

      const makePercentage = () => {
        let numbersOfExpression = [];
        const operators = ['-', '+', '*', '/'];

        for(const operator in operators) {
          if(expression.value.indexOf(operators[operator]) > 0) {
            numbersOfExpression = expression.value.split(operators[operator]);
            const percentageFromNumber = (numbersOfExpression[1] / 100) * numbersOfExpression[0];

            operation.value = expression.value + '%';
            result.value = eval(numbersOfExpression[0] + operators[operator] + percentageFromNumber);
            expression.value = '';
            break;
          }
        }
        
      }

</script>

<style lang="scss" scoped>
body {
    background: url('https://images.unsplash.com/photo-1466446105453-d151af699ac7?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1476&q=80') #ececec;
    background-size: cover;
  }
.calculator {
    background: rgba(12 38 59 / .5);
    backdrop-filter: blur(10px);
    width: 100%;
    max-width: 320px;
    min-width: 300px;
    border-radius: 20px;
    box-shadow: 0 10px 30px 1px rgba(0,0,0,.5);
    overflow: hidden;

    &__header {
        border-radius: 3px;
        padding: 30px 40px;
        position: relative;
        border-bottom: 1px solid #1a72a7;
      }
      
      &__input {
        display: block;
        width: 100%;
        box-sizing: border-box;
        border: 0;
        text-align: right;
        font-size: 30px;
        outline: none;
        background: transparent;
        color: #ffffff;
        font-weight: bold;
        
        &::placeholder {
          color: #fff;
        }
      }
      
      &__small {
        text-align: right;
        color: #a0abb2;
        font-size: 13px;
      }
      
      &__buttons {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        align-items: flex-start;
        width: 100%;
        
        &__item {
          display: block;
          width: 25%;
          height: 80px;
          background: rgba(11 39 66 / .5);
          text-align: center;
          line-height: 80px;
          cursor: pointer;
          box-sizing: border-box;
          color: #e2e9f0;
          font-size: 23px;
          
          &.operation {
            color: #4197c9;
            
            &.cancel {
              color: #a0552d;
              font-weight: bold;
            }
            
            &.result {
                position: relative;
                display: flex;
                align-items: center;
                justify-content: center;
                color: #fff;
                z-index: 1;
                font-size: 25px;
                
                &::before {
                    position: absolute;
                    left: 50%;
                    top: 50%;
                    transform: translate(-50%, -50%);
                    content: '';
                    color: #fff;
                    width: 45px;
                    height: 45px;
                    border-radius: 50%;
                    background: #2ca1fb;
                    z-index: -1;
                }
            }
            
            &.ans {
              font-size: 15px;
            }
          }
          
          &:hover {
            opacity: .7;
          }
        }
      }
}
</style>
