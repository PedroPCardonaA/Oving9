<template>
    
    <div id="container">
        <input id="sub" type="text" v-model="display">
        <input id="main" type="text" v-model="current">
        <button id="AC" @click="resetValue()">AC</button>
        <button id="ANS" @click="useANS()">ANS</button>
        <button id="DEL" @click="deleteLastDigit()" >DEL</button>
        <button id="plus" @click="defineSymbol('+')">+</button>
        <button id="zero" @click="addValue('0')">0</button>
        <button id="one" @click="addValue('1')">1</button>
        <button id="two" @click="addValue('2')" >2</button>
        <button id="three" @click="addValue('3')" >3</button>
        <button id="four" @click="addValue('4')">4</button>
        <button id="five" @click="addValue('5')">5</button>
        <button id="six" @click="addValue('6')">6</button>
        <button id="seven" @click="addValue('7')">7</button>
        <button id="eight" @:click="addValue('8')">8</button>
        <button id="nine" @click="addValue('9')">9</button>
        <button id="dot" @click="addValue('.')">.</button>
        <button id="exp" @click="defineSymbol('*10^x')">*10^x</button>
        <button id="mul" @click="defineSymbol('*')">*</button>
        <button id="div" @click="defineSymbol('/')">\</button>
        <button id="rest" @click="defineSymbol('-')">-</button>
        <button id="solve" @click="doOperation()">=</button>
        <h2 id="flashMessage" v-if="animation">Division by 0 is not possible!</h2>
            <h1 id="log">log:</h1>
            <ul id="log-list" v-show="log.length>0">
                <li v-for="index in log" :key="index.id">{{ index.text }}</li>
            </ul>
    </div>
</template>

<script>
export default {
  data(){
        return{
            ans: "0",
            current: "0",
            symbol: '',
            enteredSymbol: false,
            log: [],
            animation: false,
        };
    },
    methods:{
        addValue(value){
            if(value == '.' && this.current.includes('.')){
                value= '';
            }
            if(this.current == "0"){
                this.current = value;
            } else{
                this.current +=value;
            }
            
        },

        useANS(){
            this.current = this.ans;
        },

        resetValue(){
            this.current = 0;
        },
        deleteLastDigit(){
            if(this.current.length == 1){
                this.current = '0';
            } else{
                this.current = this.current.toString().slice(0, -1);                
            }    
        },

        doOperation(){
            if(this.enteredSymbol){
                let result = 0;
                this.ans = parseFloat(this.ans);
                this.current= parseFloat(this.current);
                if(this.symbol == '+'){
                    result = this.ans + this.current;
                } else if(this.symbol == '-'){
                    result = this.ans - this.current;
                } else if(this.symbol == '*'){
                    result = this.ans * this.current;
                } else if(this.symbol == '/'){
                    if(this.current == '0'){
                        this.animation = true
                        setTimeout(()=>{
                            this.animation = false;
                        },3000)
                        this.enteredSymbol = false;
                        this.symbol = '';
                        this.ans = result;
                        this.current = 0;
                        return;
                    }
                    result = this.ans/this.current
                } else if(this.symbol == '*10^x'){
                    result = this.ans*10**(this.current); 
                }
                this.log.unshift({ id: this.log.length + 1, text: this.ans + " " + this.symbol + " " + this.current + " = " + result});
                this.enteredSymbol = false;
                this.symbol = '';
                this.ans = result;
                this.current = 0;
            }

        },

        defineSymbol(sym){
            if(!this.enteredSymbol){
                this.ans = this.current;
            }
            this.current = 0;
            this.symbol = sym;
            this.enteredSymbol = true;
        },
    },
    computed:{
        display(){
            if(this.ans=='0' && this.symbol =='') {
                return '';
            } else{
                return this.ans + " " +this.symbol;
            }
        },
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Major+Mono+Display&display=swap');

*, html {
    margin: 0;
    padding: 0;
    font-family: 'Major Mono Display', monospace;
}

@keyframes yellowfade {
  from {
    background: #73d06f;
  }
  to {
    background: transparent;
  }
}



#container{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(5,105px);
    grid-template-rows: repeat(10,auto);
    justify-content: center;
    
}

#flashMessage {
  animation-name: yellowfade;
  animation-duration: 3s;
  grid-column: 1/-1;
  grid-row: 8/9;
}




button{
    background-color: black;
    color: #73d06f;
    width: 80px;
    height: 80px;
    font-size: x-large;
    font-family: Verdana;
    font-weight: bold;
    justify-self: center;
    align-self: start;
}

button:hover{
    cursor: crosshair;
}


#sub{
    pointer-events: none;
    cursor: default;
    background-color: black;
    color: #73d06f;
    text-align: right;
    font-size: x-large;
    font-weight: bold;
    font-family: Verdana;
    height: 50px;
    grid-column: 1/-1;
    grid-row: 2/3;
}

#main{
    pointer-events: none;
    cursor: default;
    background-color: black;
    color: #73d06f;
    text-align: right;
    font-size: x-large;
    font-weight: bold;
    font-family: Verdana;
    height: 80px;
    grid-column: 1/-1;
    grid-row: 3/4;
}


#seven{
    grid-column: 1/2;
    grid-row: 4/5;
}

#eight{
    grid-column: 2/3;
    grid-row: 4/5;
}

#nine{
    grid-column: 3/4;
    grid-row: 4/5;
}

#DEL{
    grid-column: 4/5;
    grid-row: 4/5;
}

#AC{
    grid-column: 5/6;
    grid-row: 4/5;
}

#four{
    grid-column: 1/2;
    grid-row: 5/6;
}

#five{
    grid-column: 2/3;
    grid-row: 5/6;
}

#six{
    grid-column: 3/4;
    grid-row: 5/6;
}

#mul{
    grid-column: 4/5;
    grid-row: 5/6;
}

#div{
    grid-column: 5/6;
    grid-row: 5/6;
}

#one{
    grid-column: 1/2;
    grid-row: 6/7;
}

#two{
    grid-column: 2/3;
    grid-row: 6/7;
}

#three{
    grid-column: 3/4;
    grid-row: 6/7;
}

#plus{
    grid-column: 4/5;
    grid-row: 6/7;
}

#rest{
    grid-column: 5/6;
    grid-row: 6/7;
}

#zero{
    grid-column: 1/2;
    grid-row: 7/8;
}

#dot{
    grid-column: 2/3;
    grid-row: 7/8;
}

#exp{
    grid-column: 3/4;
    grid-row: 7/8;
    font-size: medium;
}

#ANS{
    grid-column: 4/5;
    grid-row: 7/8;
}

#solve{
    grid-column: 5/6;
    grid-row: 7/8;
}

#log{
    grid-column: 1/-1;
    grid-row: 9/10;
    font-size: xx-large;
    font-weight: bolder;
    color:#73d06f;
}
#log-list{
    grid-column: 1/-1;
    grid-row: 10/11;
    font-size: xx-large;
    list-style: none;
    padding: 5px;
    list-style-position: inside;
    border: 2px solid #73d06f;
}
li{
    border: 2px solid black;
}

</style>
