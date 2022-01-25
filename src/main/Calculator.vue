<template>
   <div class="calculator">
      <Display :value="displayValue" />
      <Button label="AC" triple @button-click="clearMemory"/>
      
      <Button label="/" operation @button-click="setOperation"/>
      <Button label="7" @button-click="addDigit" />
      <Button label="8" @button-click="addDigit" />
      <Button label="9" @button-click="addDigit" />

      <Button label="*" operation @button-click="setOperation" />

      <Button label="4" @button-click="addDigit" />
      <Button label="5" @button-click="addDigit" />
      <Button label="6" @button-click="addDigit" />

      <Button label="-" operation @button-click="setOperation" />

      <Button label="1" @button-click="addDigit" />
      <Button label="2" @button-click="addDigit" />
      <Button label="3" @button-click="addDigit"/>

      <Button label="+" operation @button-click="setOperation" />
      <Button label="0" double @button-click="addDigit"/>
      <Button label="." @button-click="addDigit" />
      <Button label="=" operation @button-click="setOperation" />
   </div>
</template>

<script>
   import Display from "../components/Display.vue"
   import Button from "../components/Button.vue"

   export default {
      props: {
         kpress: {
            type: String
         }
      },
      watch: {
         kpress: {
            handler(k) {
               if (/[0-9]/i.test(k)) this.addDigit(k);
               if (/[*,/,+,-,=]/i.test(k)) this.setOperation(k);
            }
         },
      },
      data() {
         return {
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0,
         }
      },
      components: { Display, Button },
      methods: {
         clearMemory() {
            Object.assign(this.$data, this.$options.data())
         },
         setOperation(operation) {
            if (this.current === 0) {
               this.operation = operation;
               this.current = 1;
               this.clearDisplay = true;
            } else {
               const equals = operation === '=';
               const currentOperation = this.operation;

               try {
                  this.values[0] = eval(`${this.values[0]} ${currentOperation} ${this.values[1]}`);
               } catch (e) {
                  this.$emit("onError", e);
               }

               this.values[1] = 0;

               this.displayValue = this.values[0];
               this.operation = equals ? null : operation;
               this.current = equals ? 0 : 1;
               this.clearDisplay = !equals;
            }
         },
         addDigit(n) {
            if (n === '.' && this.displayValue.includes(".")) {
               return;
            }
            const clearDisplay = this.displayValue === "0" || this.clearDisplay;
            const currentValue = clearDisplay ? "" : this.displayValue;
            const displayValue = currentValue + n;


            this.displayValue = displayValue;
            this.clearDisplay = false;
            this.values[this.current] = displayValue;
         }
      },
   }

</script>

<style>
   .calculator {
      height: 520px;
      width: 90%;
      border-radius: 5px;
      overflow: hidden;
      max-width: 400px;

      display: grid;
      grid-template-columns: repeat(4, 25%);
      grid-template-rows: 0.9fr 0.6fr 0.6fr 0.6fr 0.6fr 0.6fr;
   }
</style>