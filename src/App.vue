<template>
	<div id="app">
		<div class="wrapper clearfix">
            <players
			v-bind:isWinner="isWinner"
			v-bind:scoresPlayer="scoresPlayer" 
			v-bind:activePlayer="activePlayer"
			v-bind:currentScore="currentScore"
			/>
			<controls
			v-bind:isPlaying="isPlaying"
			v-on:handleChangeFinalScore="handleChangeFinalScore"
			v-bind:finalScore="finalScore"
			v-on:handleHold="handleHold"
			v-on:handleRollDice="handleRollDice"
			v-on:handleNewGame="handleNewGame"/>
			<dices
			v-bind:dices="dices"
			/>
			<popup 
			v-on:handleConfirm="handleConfirm"
			v-bind:isOpenPopup="isOpenPopup"/>
                                  
        </div>
	</div>
</template>

<script>
import Players from './components/Players.vue'
import Controls from './components/Controls.vue'
import Dices from './components/Dices.vue'
import Popup from './components/Popup.vue'
export default {
	name: 'app',
	data () {
		return {
			isPlaying: false,
			isOpenPopup: false,
			scoresPlayer: [13, 30],
			dices: [1, 5],
			activePlayer: 1,
			currentScore: 30,
			finalScore: 10,
		}
	},
	components: {
		Players,
		Controls,
		Dices,
		Popup
	},
	computed: {
		isWinner() {
			let { scoresPlayer, finalScore } = this
			if(scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
				//dừng cuộc chơi
				this.isPlaying = false
				return true
			}
			return false

		}
	},
	methods: {
		handleNewGame() {
			// console.log('handlenewgame tu app')
			this.isOpenPopup = true;
			this.isPlaying= true,
			this.scoresPlayer= [0, 0],
			this.activePlayer= 0,
			this.currentScore= 0
			this.dices= [1, 2]
		},
		handleConfirm() {
			this.isOpenPopup = false;
		},
		handleRollDice() {
			// console.log('Rolldice from App')
			if(this.isPlaying) {
				// vì math.random chỉ random số lẻ từ 0 -> 1 => phải x 6 để dc >= 6
				var dice1 = Math.floor(Math.random() * 6) + 1
				var dice2 = Math.floor(Math.random() * 6) + 1
				this.dices = [dice1, dice2]
				if(dice1 == 1 || dice2 == 1) {
					//đổi lượt chơi
					//reset current score về 0
					setTimeout(() => {
						alert('Mất quyền chơi')
					}, 10);					
					this.activePlayer = this.activePlayer === 0 ? 1 : 0
					this.currentScore = 0
					this.dices= [1, 1]
				} else {
					this.currentScore = this.currentScore + dice1 + dice2
				}
			} else {
				alert('Vui lòng nhấn New Game')
			}
		},
		handleHold() {
			if(this.isPlaying) {
				// ta có scorePlayer của player index 0 
				// ta có scorePlayer của player index 1
				//tương đương scorePlayer[activePlayer] = điểm của player 
				var scoresPlayerPresent = this.scoresPlayer[this.activePlayer]
				this.$set(this.scoresPlayer, this.activePlayer, scoresPlayerPresent + this.currentScore)

				if(!this.isWinner) {
					this.activePlayer = this.activePlayer === 0 ? 1 : 0
					this.currentScore = 0
					this.dices= [1, 1]
				}
			} else {
				alert('Vui lòng nhấn New Game')
			}

			console.log('handleHold from App')
		},
		handleChangeFinalScore(e) {
			var number = parseInt(e.target.value)
			if(isNaN(number)) {
				this.finalScore = ''
			} else {
				this.finalScore = number
			}
		}
	}
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}
</style>





