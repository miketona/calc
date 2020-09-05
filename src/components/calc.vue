<template>
  <div class="calc">
    <div class="screen">{{screenVal || 0}}</div>
    <div @click="clear" class="btn">C</div>
    <div @click="negate" class="btn">+/-</div>
    <div @click="getPercentage" class="btn">%</div>
    <div @click="concatOperator('/')" class="btn">/</div>
    <div @click="concatNum('7')" class="btn num">7</div>
    <div @click="concatNum('8')" class="btn num">8</div>
    <div @click="concatNum('9')" class="btn num">9</div>
    <div @click="concatOperator('*')" class="btn">*</div>
    <div @click="concatNum('4')" class="btn num">4</div>
    <div @click="concatNum('4')" class="btn num">5</div>
    <div @click="concatNum('6')" class="btn num">6</div>
    <div @click="concatOperator('-')" class="btn">-</div>
    <div @click="concatNum('1')" class="btn num">1</div>
    <div @click="concatNum('2')" class="btn num">2</div>
    <div @click="concatNum('3')" class="btn num">3</div>
    <div @click="concatOperator('+')" class="btn">+</div>
    <div @click="concatNum('0')" class="btn num double">0</div>
    <div @click="concatNum('.')" class="btn num">.</div>
    <div @click="doMath" class="btn">=</div>
    <div id="errorMessage">{{errorMessage || ""}}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      screenVal: "", //value of the number that is displayed on the screen.
      operatorState: "", // Keeps track of opeator in progress
      errorMessage: "",
    };
  },
  methods: {
    getSecondVal() {
      const middle = this.screenVal.indexOf(this.operatorState);
      const valOne = this.screenVal.substring(0, middle);
      const valTwo = this.screenVal.substring(
        middle + 1,
        this.screenVal.length
      );
      this.screenVal = valOne + this.operatorState;
      return valTwo;
    },
    clear() {
      this.screenVal = "";
      this.operatorState = "";
      this.errorMessage = "";
    },
    negate() {
      if (!this.screenVal)
        return (this.errorMessage =
          "Please pick a number before trying to change the sign");
      if (!this.operatorState) return (this.screenVal *= -1);
      //if operator state, then it must be the 2nd number. Get second number and negate it.
      const secondVal = this.getSecondVal();
      if (!secondVal)
        return (this.errorMessage = "Please enter a second value first");
      console.log(secondVal);
      this.screenVal += secondVal * -1;
    },
    getPercentage() {
      if (this.operatorState || !this.screenVal)
        return (this.errorMessage = "Not a valid use of %");
      this.screenVal = parseFloat(this.screenVal) / 100;
    },
    concatNum(num) {
      if (num === "." && this.screenVal.includes("."))
        if (!this.operatorState)
          //first val already has .
          return (this.errorMessage =
            "This element can not contain a second .");
        else {
          let secondVal = this.getSecondVal();
          //second val already has .
          this.screenVal += secondVal;
          if (secondVal.includes("."))
            return (this.errorMessage =
              "This element can not contain a second .");
        }
      this.screenVal += num;
    },
    concatOperator(operator) {
      if (this.operatorState) return (this.errorMessage = "Not valid");
      if (!this.screenVal) {
        this.screenVal += "0" + operator;
        this.operatorState = operator;
      } else {
        this.screenVal += operator;
        this.operatorState = operator;
      }
    },
    doMath() {
      if (!this.operatorState)
        return (this.errorMessage =
          "Please select an operator and a second number first");

      this.screenVal = eval(this.screenVal); //This function is so cool :D
      this.errorMessage = "";
      this.operatorState = "";
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calc {
  margin: 0 auto;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 440px;
  background-color: blanchedalmond;
}
.screen {
  width: 100%;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: grey;
  color: white;
}
.btn {
  width: 24.5%;
  border: 1.1px solid black;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.btn.num {
  background-color: beige;
}
.double {
  width: 49.5%;
}
#errorMessage {
  height: 20px;
  background-color: white;
  width: 100%;
  color: red;
}
</style>
