<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Hello Vue 3.0 + Vite" /> -->
  <div id="calculator">
    <div id="preview">
      <div class="sequence">{{" "+getSeq()}}</div>
      <span>{{getNum()}}</span>
    </div>

    <div id="pad">
      <div id="control-pad">
        <div @click="clear()" class="calc-btn" id="clear">AC</div>
      </div>
      <div id="number-pad">
        <div @click="appendNum(0)" class="calc-btn" id="zero">0</div>
        <div @click="appendNum('.')" class="calc-btn" id="decimal">.</div>
        <div @click="appendNum(1)" class="calc-btn" id="one">1</div>
        <div @click="appendNum(2)" class="calc-btn" id="two">2</div>
        <div @click="appendNum(3)" class="calc-btn" id="three">3</div>
        <div @click="appendNum(4)" class="calc-btn" id="four">4</div>
        <div @click="appendNum(5)" class="calc-btn" id="five">5</div>
        <div @click="appendNum(6)" class="calc-btn" id="six">6</div>
        <div @click="appendNum(7)" class="calc-btn" id="seven">7</div>
        <div @click="appendNum(8)" class="calc-btn" id="eight">8</div>
        <div @click="appendNum(9)" class="calc-btn" id="nine">9</div>
      </div>
      <div id="operation-pad">
        <div @click="appendOperand('÷')" class="calc-btn" id="divide">÷</div>
        <div @click="appendOperand('x')" class="calc-btn" id="multiply">x</div>
        <div @click="appendOperand('-')" class="calc-btn" id="substract">-</div>
        <div @click="appendOperand('+')" class="calc-btn" id="add">+</div>
        <div @click="appendOperand('=')" class="calc-btn" id="equals">=</div>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    // HelloWorld,
  },
  data: () => {
    return {
      seq: [],
      num: "0",
    };
  },

  methods: {
    getNum() {
      return Number.parseFloat(this.num);
    },
    getSeq() {
      return this.seq.join(" ");
    },
    appendNum(num) {
      if (this.num.indexOf(".") >= 0 && num == ".") return;

      this.num += num;
    },
    appendOperand(operand) {
      if (this.seq.indexOf("=") >= 0) {
        // this.seq = [];
        this.seq = [];
        // this.num = "0";
      }
      this.seq.push(this.getNum());
      this.seq.push(operand);
      if (operand == "=") {
        const res = this.calculate().toString();
        this.num = res;
        return;
      }
      this.num = "0";
    },
    calculate() {
      let result = 0;
      for (let index = 0; index < this.seq.length; index += 3) {
        const num1 = this.seq[index];
        const operand = this.seq[index + 1];
        const num2 = this.seq[index + 2];
        switch (operand) {
          case "÷":
            result += num1 / num2;
            break;
          case "x":
            result += num1 * num2;
            break;
          case "-":
            result += num1 - num2;
            break;
          case "+":
            result += num1 + num2;
            break;
          default:
            break;
        }
      }
      return result;
    },
    clear() {
      this.seq = [];
      this.num = "0";
    },
  },
};
</script>

<style>
:root {
  --calc-width: 400px;
  --numpad-width: 300px;
  --btn-height: 12vh;
  --btn-selected-offset: 1px;
}
#calculator {
  display: flex;
  flex-flow: column wrap;
  align-items: flex-start;
}
#pad {
  width: 400px;
  display: flex;
  flex-flow: column wrap;
  height: calc(var(--btn-height) * 5);
  /* border: 1px solid black; */
  /* align-items: flex-start; */
}
#preview {
  width: var(--calc-width);

  border: 1px solid black;
  border-bottom: none;
  text-align: end;
  font-size: 30px;
  box-sizing: border-box;
  padding: 5px;
}
#preview .sequence {
  flex: 1;
  text-align: end;
  text-decoration: underline dotted;
  font-size: initial;
}
#preview span {
  flex: 2;
  width: fit-content;
}

#control-pad {
  width: var(--numpad-width);
  display: flex;
  flex-flow: row nowrap;
}
#control-pad .calc-btn {
  flex: 1;
}
#number-pad {
  display: flex;
  flex-flow: row wrap-reverse;
  width: var(--numpad-width);
}
#operation-pad {
  color: white;

  font-size: 30px;
  width: calc(var(--calc-width) - var(--numpad-width));
  flex-wrap: wrap;
}
#operation-pad .calc-btn {
  background-color: orange;
  padding: -5px;

  /* margin: 10px; */
}
#operation-pad .calc-btn:hover {
  background-color: rgb(255, 199, 96);
}

.calc-btn {
  width: calc(var(--numpad-width) / 3);
  height: var(--btn-height);
  box-sizing: border-box;
  line-height: calc(var(--btn-height) - 2px);
  border: 1px solid black;
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  border-collapse: collapse;
  transition: background-color 0.1s cubic-bezier(0.785, 0.135, 0.15, 0.86);
  /* transition: all 0.3s linear; */
}
.calc-btn#zero {
  width: calc(var(--numpad-width) / 3 * 2);
}
.calc-btn:hover {
  /* filter: saturate(1); */
  transition: all 0.3s cubic-bezier(0.785, 0.135, 0.15, 0.86);
  /* transition: all 0.3s linear; */
}
.calc-btn-selected {
  box-shadow: 2px 2px black inset, -2px -2px black inset;
}
.calc-btn:active {
  filter: brightness(1.1);
  box-shadow: 5px 5px lightgrey inset, -5px -5px lightgrey inset;
  transition: all 1ms cubic-bezier(0.445, 0.05, 0.55, 0.95);
  /* transition: all 1ms linear; */
}
</style>

