<script setup>
import { callWithAsyncErrorHandling, ref } from 'vue';

// Calculator

let screenNumber = ref('0')
let currentOperation = ref(null)
let previousNum = ref(null)

function addNumber(num) {
  if(screenNumber.value === '0') {
    screenNumber.value = num
  } else {
    screenNumber.value += num
  }
}

function removeLastNumber() {
  if(screenNumber.value.length <= 1 ){
    screenNumber.value = '0'
    return
  }
  screenNumber.value = screenNumber.value.slice(0,-1)
}

function eraseAll() {
  screenNumber.value = '0'
}

function handleOperation(operation) {
  if(currentOperation.value) {
    calculate()
  }
  previousNum.value = parseInt(screenNumber.value)
  currentOperation.value = operation
  screenNumber.value = '0'
}

function calculate(previousOperation) {
  if(currentOperation.value && previousNum.value != null) {
    const currentValue = parseInt(screenNumber.value)
    let result
    switch(currentOperation.value) {
      case '+':
        result = previousNum.value + currentValue
        break
      case '-':
        result = previousNum.value - currentValue
        break
      case '*':
        result = previousNum.value * currentValue
        break
      case '/':
        result = previousNum.value / currentValue
        break
    }
    screenNumber.value = result.toString()
    currentOperation.value = null
    previousNum.value = null
  }
}

// STYLE

const isHovered = ref(false);

function handleMouseOver() {
  isHovered.value = true;
}

function handleMouseLeave() {
  isHovered.value = false;
}

</script>

<template>
  <div 
  :class="{'hovered': isHovered}"
  @mouseover="handleMouseOver"
  @mouseleave="handleMouseLeave"
  class="wrapper"
  >
    <section 
    class="screen"
    :class="{'screen-hovered': isHovered}"
    >
      {{ screenNumber }}
    </section>

    <section 
      class="calc-btns"
    >
      <div class="calc-btn-row">
        <button @click="eraseAll" class="btn-calc double" :class="{'btn-hovered': isHovered}">
          C
        </button>
        <button @click="removeLastNumber" class="btn-calc" :class="{'btn-hovered': isHovered}">
          &larr;
        </button>
        <button @click="handleOperation('/')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          &divide;
        </button>
      </div>

      <div class="calc-btn-row">
        <button @click="addNumber('7')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          7
        </button>
        <button @click="addNumber('8')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          8
        </button>
        <button @click="addNumber('9')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          9
        </button>
        <button @click="handleOperation('*')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          &times;
        </button>
      </div>

      <div class="calc-btn-row">
        <button @click="addNumber('4')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          4
        </button>
        <button @click="addNumber('5')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          5
        </button>
        <button @click="addNumber('6')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          6
        </button>
        <button @click="handleOperation('-')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          &minus;
        </button>
      </div>

      <div class="calc-btn-row">
        <button @click="addNumber('1')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          1
        </button>
        <button @click="addNumber('2')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          2
        </button>
        <button @click="addNumber('3')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          3
        </button>
        <button @click="handleOperation('+')" class="btn-calc" :class="{'btn-hovered': isHovered}">
          &plus;
        </button>
      </div>
      
      <div class="calc-btn-row">
        <button @click="addNumber('0')" class="btn-calc triple" :class="{'btn-hovered': isHovered}">
          0
        </button>
        <button @click="calculate" class="btn-calc" :class="{'btn-hovered': isHovered}">
          &equals;
        </button>
      </div>
    </section>
  </div>
</template>

<style scoped>
  .wrapper{
    border-radius: 16px;
    color: #232323;
    background: #1B263B;
    flex-basis: 400px;
    height: 540px;
    padding: 20px 35px;
    transition: all .5s;
  }

  .screen{
    backdrop-filter: blur(5.5px);
    -webkit-backdrop-filter: blur(5.5px);
    background: rgba(255, 255, 255, 0.479);
    border: 1px solid rgba(255, 255, 255, 0.01);
    border-radius: 16px;
    color: #232323;
    font-size: 35px;
    overflow: auto;
    padding: 20px;
    text-align: right;
    width: 326px;
    transition: all .5s;
  }

  .calc-btn-row{
    display: flex;
    justify-content: space-between;
    margin: 5% 0;
  }

  .btn-calc{
    backdrop-filter: blur(5.5px);
    -webkit-backdrop-filter: blur(5.5px);
    background: #415A77;
    border: 1px solid rgba(255, 255, 255, 0.01);
    border-radius: 16px;
    color: #ccc;
    flex-basis: 20%;
    font-family: inherit;
    font-size: 24px;
    height: 65px;
    cursor: pointer;
    transition: all .3s;
  }

  .btn-calc:last-child{
    backdrop-filter: blur(5.5px);
    -webkit-backdrop-filter: blur(5.5px);
    background: rgba(255, 255, 255, 0.75);
    border-radius: 16px;
    color: #fff;
    background: #0D1B2A;
  }

  .btn-calc:last-child:hover{
    background-color: #E0E1DD;
    color: inherit;
  }

  .btn-calc:hover{
    background-color: #E0E1DD;
    color: inherit;
    border-right: 4px solid #aaaaaa;
    border-bottom: 2px solid #aaaaaa;
    border-left: 4px solid #E0E1DD;
    border-top: 2px solid #E0E1DD;
  }

  .btn-calc:active{
    box-shadow: 3px 3px 20px #fff;
  }

  .hovered{
    border-right: 6px solid #0D1B2A;
    border-bottom: 3px solid #0D1B2A;
    border-left: 6px solid #415A77;
    border-top: 3px solid #415A77;
    box-shadow: 40px 20px 70px #000;
  }

  .screen-hovered{
    box-shadow: 1px 1px 15px #fff;
    background: #E0E1DD;
    border-top: 3px solid #ccc;
    border-left: 6px solid #ccc;
  }

  .btn-hovered{
    box-shadow: 3px 1px 10px #000;
    border-left: 3px solid #778DA9;
    border-top: 1px solid #778DA9;
  }

  .btn-hovered:last-child{
    box-shadow: 2px 2px 20px #000;
    border-left: 4px solid #1B263B;
    border-top: 2px solid #1B263B;
  }

  .double{
    flex-basis: 47%;
  }
  
  .triple{
    flex-basis: 73%;
  }
</style>
