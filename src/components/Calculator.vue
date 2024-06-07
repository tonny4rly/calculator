<template>
    <div id="app">
      <div class="title">
        <h1>Examen vuejs : Calculatrice<br><span class="about">BEMAHARESY Arly Tonny<br>45/LA/23-24</span></h1>
      </div>
  
      <div class="calculator">
        <input type="text" class="calculator-screen" :value="calculatorValue" disabled />
        <div class="calculator-keys">
          <button v-for="button in buttons" :key="button.value" type="button" :class="[button.type]" @click="handleButtonClick(button.value)">
            {{ button.display }}
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script>

  export default {

    name: 'CalculatorApp',

    data() {
      return {
        calculatorValue: '0',
        firstOperand: null,
        awaitingSecondOperand: false,
        operator: null,
        buttons: [
          { type: 'function', value: 'sin', display: 'sin' },
          { type: 'function', value: 'cos', display: 'cos' },
          { type: 'function', value: 'tan', display: 'tan' },
          { type: 'function', value: 'log', display: 'log' },
          { type: 'function', value: 'sqrt', display: '√' },
          { type: 'operator', value: '+', display: '+' },
          { type: 'operator', value: '-', display: '-' },
          { type: 'operator', value: '*', display: '×' },
          { type: 'operator', value: '/', display: '÷' },
          { type: 'operator', value: '%', display: '%' },
          { type: '', value: '7', display: '7' },
          { type: '', value: '8', display: '8' },
          { type: '', value: '9', display: '9' },
          { type: '', value: '4', display: '4' },
          { type: '', value: '5', display: '5' },
          { type: '', value: '6', display: '6' },
          { type: '', value: '1', display: '1' },
          { type: '', value: '2', display: '2' },
          { type: '', value: '3', display: '3' },
          { type: '', value: '0', display: '0' },
          { type: 'decimal', value: '.', display: '.' },
          { type: '', value: '(', display: '(' },
          { type: '', value: ')', display: ')' },
          { type: 'all-clear', value: 'all-clear', display: 'AC' },
          { type: 'equal-sign operator', value: '=', display: '=' },
        ]
      };
    },
    methods: {
      handleButtonClick(value) {
        switch (value) {
          case '+':
          case '-':
          case '*':
          case '/':
          case '%':
          case '=':
            this.handleOperator(value);
            break;
          case 'sin':
          case 'cos':
          case 'tan':
          case 'log':
          case 'sqrt':
            this.handleFunction(value);
            break;
          case '.':
            this.enterDecimal(value);
            break;
          case '(':
          case ')':
            this.enterCharacter(value);
            break;
          case 'all-clear':
            this.resetCalculator();
            break;
          default:
            if (!isNaN(parseFloat(value))) {
              this.enterNumber(value);
            }
        }
      },
      enterNumber(number) {
        if (this.awaitingSecondOperand) {
          this.calculatorValue = String(number);
          this.awaitingSecondOperand = false;
        } else {
          this.calculatorValue = this.calculatorValue === '0' ? String(number) : this.calculatorValue + String(number);
        }
      },
      enterDecimal(point) {
        if (this.awaitingSecondOperand) {
          this.calculatorValue = '0.';
          this.awaitingSecondOperand = false;
          return;
        }
  
        if (!this.calculatorValue.includes(point)) {
          this.calculatorValue += point;
        }
      },
      enterCharacter(character) {
        this.calculatorValue += character;
      },
      handleOperator(nextOperator) {
        const { firstOperand, calculatorValue, operator } = this;
        const inputValue = parseFloat(calculatorValue);
  
        if (operator && this.awaitingSecondOperand) {
          this.operator = nextOperator;
          return;
        }
  
        if (firstOperand == null && !isNaN(inputValue)) {
          this.firstOperand = inputValue;
        } else if (operator) {
          const result = this.calculate(firstOperand, inputValue, operator);
  
          this.calculatorValue = `${parseFloat(result.toFixed(7))}`;
          this.firstOperand = result;
        }
  
        this.awaitingSecondOperand = true;
        this.operator = nextOperator;
      },
      calculate(firstOperand, secondOperand, operator) {
        switch (operator) {
          case '+':
            return firstOperand + secondOperand;
          case '-':
            return firstOperand - secondOperand;
          case '*':
            return firstOperand * secondOperand;
          case '/':
            return firstOperand / secondOperand;
          case '%':
            return firstOperand % secondOperand;
          default:
            return secondOperand;
        }
      },
      handleFunction(func) {
        let result = parseFloat(this.calculatorValue);
  
        switch (func) {
          case 'sin':
            result = Math.sin(result);
            break;
          case 'cos':
            result = Math.cos(result);
            break;
          case 'tan':
            result = Math.tan(result);
            break;
          case 'log':
            result = Math.log10(result);
            break;
          case 'sqrt':
            result = Math.sqrt(result);
            break;
        }
  
        this.calculatorValue = `${parseFloat(result.toFixed(7))}`;
        this.awaitingSecondOperand = true;
      },
      resetCalculator() {
        this.calculatorValue = '0';
        this.firstOperand = null;
        this.awaitingSecondOperand = false;
        this.operator = null;
      }
    }
  };
  </script>
  
  <style scoped>
body {
  font-family: 'Roboto', sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: 0;
  padding: 40px 0 40%;
  background: linear-gradient(#fbf5f5, rgba(128, 128, 128, 0.65));
}

.title h1 {
  font-family: Georgia, 'Times New Roman', Times, serif;
  font-size: 40px;
  background: linear-gradient(to right, rgb(205, 133, 1), rgb(172, 2, 2));
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
  margin-bottom: 50px;
  text-align: center;
}

.title h1 .about {
  font-size: 25px;
}

.calculator {
  width: 500px;
  margin: 0 auto;
  border-radius: 15px;
  overflow: hidden;
  box-shadow: 0px 0px 20px 0px #0000001a;
  padding: 30px 15px 20px;
  background-color: #353232;
}

.calculator-screen {
  width: calc(100% - 28px);
  height: 80px;
  border: none;
  background-color: #837e7e;
  color: #fff;
  text-align: right;
  padding-right: 20px;
  padding-left: 10px;
  font-size: 2.5rem;
  border-radius: 10px;
}

button {
  height: 60px;
  font-size: 1.4rem;
  flex: 1 1 20%;
  cursor: pointer;
  border-radius: 10px;
  border: none;
}

button:hover {
  background: #d7d5d5;
}

.operator,
.function {
  background-color: #f8a33c;
  color: white;
}

.operator:hover,
.function:hover {
  background-color: #d58b30;
}

.equal-sign {
  height: 100%;
  grid-column: span 2;
  background-color: #f8573c;
}

.equal-sign:hover {
  background-color: #cd3f26;
}

.all-clear {
  background-color: #f95c2c;
  color: white;
}

.all-clear:hover {
  background-color: #c64017;
}

.calculator-keys {
  display: grid;
  margin-top: 30px;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  padding: 20px;
  background: #ffffff;
  border-radius: 10px;
}

.calculator-keys button:nth-last-child(1) {
  grid-column: 2 / 4;
  height: 40px;
}

@media (max-width: 768px) {
  .title h1 {
    font-size: 30px;
    margin-bottom: 30px;
  }

  .title h1 .about {
    font-size: 20px;
  }

  .calculator {
  width: 300px;
  }

  .calculator-screen {
    width: calc(100% - 20px);
    height: 60px;
    font-size: 2rem;
    padding-right: 15px;
    padding-left: 5px;
  }

  button {
    height: 50px;
    font-size: 1.2rem;
  }
}

@media (max-width: 480px) {
  .title h1 {
    font-size: 25px;
    margin-bottom: 20px;
  }

  .title h1 .about {
    font-size: 15px;
  }

  .calculator-screen {
    width: calc(100% - 15px);
    height: 50px;
    font-size: 1.5rem;
    padding-right: 10px;
    padding-left: 5px;
  }

  button {
    height: 40px;
    font-size: 1rem;
  }

  .equal-sign {
    grid-column: span 2;
  }

  .calculator {
    padding: 20px 10px 15px;
  }
}

@media (max-width: 480px){
    .calculator{
        width: 260px;
    }
}
@media (max-width: 295px){
    .calculator{
        width: 200px;
    }
}


</style>
