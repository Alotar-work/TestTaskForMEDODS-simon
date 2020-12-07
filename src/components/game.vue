<template>
    <div class="container">
        <div class="gamePanel">
            <table>
                <tr>
                    <td><button class="gameButton buttonBlue" v-bind:class="{ buttonBlueLight: isButtonBlueClick == true }" v-on:click="buttonBlueClick()" :disabled="!isStartRound"></button></td>
                    <td><button class="gameButton buttonRed" v-bind:class="{ buttonRedLight: isButtonRedClick == true }" v-on:click="buttonRedClick()" :disabled="!isStartRound"></button></td>
                </tr>
                <tr>
                    <td><button class="gameButton buttonYellow" v-bind:class="{ buttonYellowLight: isButtonYellowClick == true }" v-on:click="buttonYellowClick()" :disabled="!isStartRound"></button></td>
                    <td><button class="gameButton buttonGreen" v-bind:class="{ buttonGreenLight: isButtonGreenClick == true }" v-on:click="buttonGreenClick()" :disabled="!isStartRound"></button></td>
                </tr>
            </table>
        </div>
        <div class="row">
            <div>Сложность: </div>
            <div class="complexity"><input name="complexity" type="radio" value="1500" checked v-model="time"> Легкий</div>
            <div class="complexity"><input name="complexity" type="radio" value="1000" v-model="time"> Средний</div>
            <div class="complexity"><input name="complexity" type="radio" value="400" v-model="time"> Сложный</div>
            <button class="startButton" v-on:click="startGame()">Начать</button>
            <div class="complexity">Раунд: {{round}}</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'game',
    props: {
    },
    data() {
        return{
            isButtonBlueClick: false,
            isButtonRedClick: false,
            isButtonYellowClick: false,
            isButtonGreenClick: false,
            time: 1500,
            timer: 1500,
            isStartRound: false,
            sound: [],
            currentNote: 0,
            round: 0
        }
    },
    methods:{
        buttonBlueClick(){
            this.isButtonBlueClick = true
            var audio = new Audio(require('@/assets/sounds/1.mp3'))
            if(!this.isStartRound){
                setTimeout(()=>{this.isButtonBlueClick = false }, this.timer)
            }else{
                setTimeout(()=>{this.isButtonBlueClick = false }, 200)
                this.cheack(0)                
            }
            audio.play()   
        },
        buttonRedClick(){
            this.isButtonRedClick = true
            var audio = new Audio(require('@/assets/sounds/2.mp3'))
            if(!this.isStartRound){
                setTimeout(()=>{this.isButtonRedClick = false }, this.timer)
            }else{
                setTimeout(()=>{this.isButtonRedClick = false }, 200)
                this.cheack(1)
            }
            audio.play()
        },
        buttonYellowClick(){
            this.isButtonYellowClick = true
            var audio = new Audio(require('@/assets/sounds/3.mp3'))
            if(!this.isStartRound){
                setTimeout(()=>{this.isButtonYellowClick = false }, this.timer)
            }else{
                setTimeout(()=>{this.isButtonYellowClick = false }, 200)
                this.cheack(2)
            }
            audio.play()
        },
        buttonGreenClick(){
            this.isButtonGreenClick = true
            var audio = new Audio(require('@/assets/sounds/4.mp3'))
            if(!this.isStartRound){
                setTimeout(()=>{this.isButtonGreenClick = false }, this.timer)
            }else{
                setTimeout(()=>{this.isButtonGreenClick = false }, 100)
                this.cheack(3)
            }
            audio.play()
        },
        startGame(){
            this.timer = this.time
            this.sound = []
            this.round++
            this.addNote()
        },
        addNote(){
            this.currentNote = 0
            this.sound.push(Math.floor(Math.random() * 4))
            this.playSound()
        },
        playSound(){
            if(this.sound[this.currentNote] == 0){
                this.buttonBlueClick()
            }else if(this.sound[this.currentNote] == 1){
                this.buttonRedClick()
            }else if(this.sound[this.currentNote] == 2){
                this.buttonYellowClick()
            }else if(this.sound[this.currentNote] == 3){
                this.buttonGreenClick()
            }

            if(this.currentNote + 1 != this.sound.length){
                this.currentNote++
                setTimeout(()=>{this.playSound()},this.timer)                
            }else{
                this.currentNote = 0;
                this.isStartRound = true
            }
        },
        cheack(noteNumber){
            if(this.sound[this.currentNote] != noteNumber){
                this.isStartRound = false
                setTimeout(()=>{alert("Неверно! Игра окончена. Пройдено раундов: " + (this.round - 1)); this.round = 0 }, 200)                
            }else if(this.currentNote + 1 == this.sound.length){                
                this.isStartRound = false
                setTimeout(()=>{this.round++; this.addNote() }, 1000)                
            }else{
                this.currentNote++
            }
        }
    }
}
</script>

<style>
</style>