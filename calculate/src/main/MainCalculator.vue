<template>
  <div class="calculate">
    <CalDisplay value="" />
    <CalButton label="AC" triple @onClick="clearMemory" />
    <CalButton label="/" operation @onClick="setOperation" />
    <CalButton label="7" @onClick="addDigit" />
    <CalButton label="8" @onClick="addDigit" />
    <CalButton label="9" @onClick="addDigit" />
    <CalButton label="*" operation @onClick="setOperation" />
    <CalButton label="4" @onClick="addDigit" />
    <CalButton label="5" @onClick="addDigit" />
    <CalButton label="6" @onClick="addDigit" />
    <CalButton label="-" operation @onClick="setOperation" />
    <CalButton label="1" @onClick="addDigit" />
    <CalButton label="2" @onClick="addDigit" />
    <CalButton label="3" @onClick="addDigit" />
    <CalButton label="+" operation @onClick="setOperation" />
    <CalButton label="0" @onClick="addDigit" double />
    <CalButton label="." @onClick="addDigit" />
    <CalButton label="=" operation @onClick="setOperation" />
  </div>
</template>

<script>
import CalDisplay from "@/components/CalDisplay.vue";
import CalButton from "@/components/CalButton.vue";
export default {
  name: "MainCalculator",
  data() {
    return {
      displayValue: "0",
      clearDisplay: false,
      operation: null,
      value: [0, 0],
      current: 0,
    };
  },
  components: {
    CalButton,
    CalDisplay,
  },
  methods: {
    clearMemory() {
      Object.assign(this.$data, this.$options.data())
    },
    setOperation(operation) {
      if(this.current === 0){
        this.operation = operation
        this.current = 1
        this.clearDisplay = true
      }else{
        const equals = operation === '='
        const currentOperation = this.operation

        try{
          this.value[0] = eval(
            `${this.values[0]} ${currentOperation} ${this.values[1]}`
          )
        }catch(e){
          this.$emit('onError', e)
        }

        this.values[1] = 0
        this.displayValue = this.values[0]
        this.operation = equals ? null : operation
        this.current = equals ? 0 : 1
        this.clearDisplay = !equals
      }
    },
    addDigit(n) {
      if (n === '' && this.displayValue.includes('.')){
        return
      }

      const clearDisplay = this.displayValue === '0' || this.clearDisplay
      const currentValue = clearDisplay ? "" : this.displayValue
      const displayValue = currentValue + n

      this.displayValue = displayValue
      this.clearDisplay = false
    },
  },
};
</script>

<style>
.calculate {
  height: 320px;
  width: 255px;
  border-radius: 5px;
  overflow: hidden;

  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-rows: 1fr 48px 48px 48px 48px;
}
</style>
