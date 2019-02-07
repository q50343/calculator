<template>
  <div id="app">
    <div class="calculator">
      <div class="head">
        <div class="all"></div>
        <p>{{all}}</p>
        <h1 :style="`fontSize:${fz}px`">{{displayInput}}</h1>
      </div>
      <div class="container">
        <div class="nums center" v-for='num in nums' :key='num' :style='`grid-area:a${num}`' @click='append(num)'>{{num}}</div>
        <div class="nums center" @click='dot()' style='grid-area:a'>.</div>
        <div class="sign center" @click='operator("/")'>÷</div>
        <div class="sign center" @click='operator("*")'>×</div>
        <div class="sign center" @click='operator("+")'>+</div>
        <div class="sign center" @click='operator("-")'>−</div>
        <div class="blue center" @click="clear()">AC</div>
        <div class="blue center" @click="back()">⌫</div>
        <div class="equal" @click="equal()">=</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      nums:[1,2,3,4,5,6,7,8,9,'0','00'],
      current: 0,
      all:0,
      previous: 0,
    }
  },
  computed:{
    displayInput() {
      if (isNaN(this.current)) {
        let str = this.current
          .replace(/\*/g, " × ")
          .replace(/\//g, " ÷ ")
          .split(" ")
        return str.map(s => this.numberWithCommas(s)).join(" ")
      } else {
        return this.numberWithCommas(this.current)
      }
    },
    fz() {
      let length = this.current.toString().length
      if (length >= 24) return 20
      else if (length >= 20) return 24
      else if (length >= 15) return 27
      else if (length >= 12) return 36
      else if (length >= 10) return 45
      return 56
    },
  },
  methods:{
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
    },
    append (num) {
      if (!this.current) {
        this.current = ''
      }
      this.current += num
    },
    clear () {
      this.current = 0
      this.all = 0
    },
    dot(){
      this.current.indexOf('.') === -1? this.append('.'):''
    },
    operator(sign){
      this.current = this.current.toString()
      if(isNaN(Number(this.current.slice(-1)))){
        this.current = this.current.slice(0,-1)
      }
      this.current += sign 
    },
    back(){
      this.current = this.current.slice(0,-1)
    },
    equal () {
      this.all = this.displayInput
      this.current = eval(this.current)
    }
  }
  
}
</script>

<style>
html,body{
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100%;
  background: #E8E8E8;
  display: flex;
  justify-content: center;
  align-items: center; 
  font-family: 'Roboto', sans-serif;
}
*{
  transition: .3s
}
.calculator{
  box-shadow: 0px 20px 40px rgba(0,0,0,.4);
  border-radius: 20px
}
.head{
  background: #041936;
  border-radius: 20px 20px 0px 0px;
  width: 350px;
  height: 109px;
  position: relative;
}
h1{
  color: #fff;
  bottom: 0;
  right:0;
  font-size: 56px;
  margin: 0;
  padding-right: 16px;
  font-weight: normal;
  text-align: right;
  line-height: 56px;
}
p{
  margin: 0;
  padding:16px 20px 0 0;
  color: #00C4FF;
  text-align: right;
}
.container{
  width: 350px;
  height: 416px;
  padding: 16px 8px;
  display: grid;
  background: #062145;;
  box-sizing: border-box;
  grid-template-columns: 1fr 1fr 1fr 72px;
  grid-template-rows: 64px 64px 64px 64px 64px;
  border-radius: 0 0 20px 20px;
  grid-gap: 16px;
  grid-template-areas: 
    'a7 a8 a9 b'
    'a4 a5 a6 b'
    'a1 a2 a3 b'
    'a0 a00 a b'
    'c c d .'
}
.center{
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  cursor: pointer;
}
.center:hover{
  background: #00C4FF;
  border-radius: 16px
}
.nums{ 
  color: #fff; 
}
.sign{
  color: #fff;
  background: #041936;
  border-radius: 16px;
}
.blue{
  color: #00C4FF;
}
.blue:hover{
  color: #fff;
}
.equal{
  color: #fff;
  background: linear-gradient(90deg,#00C4FF,#6C00FF);
  border-radius: 16px;
  grid-column: 3/5;
  text-align: right;
  font-size: 24px;
  line-height: 64px;
  padding-right: 32px;
  box-sizing: border-box;
  max-width: 100%;
  cursor: pointer;
}
.equal:hover{
  background: linear-gradient(270deg,#00C4FF,#6C00FF);
}
</style>
