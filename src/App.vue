<template>
	<div class="wrapper">
		<h1>Simon Says</h1>
			<div class="game-board">
				<div class="simon">
					<!-- <p>{{order.join(' ')}}</p> -->
					<ul>
						<li class="tile red" :style="{background: red ? 'red' : ''}" data-tile="1" @click="oneClick"></li>
						<li class="tile blue" :style="{background: blue ? 'blue' : ''}" data-tile="2" @click="twoClick"></li>
						<li class="tile yellow" :style="{background: yellow ? 'yellow' : ''}" data-tile="3" @click="threeClick"></li>
						<li class="tile green" :style="{background: green ? 'green' : '' }" data-tile="4" @click="fourClick"></li>
					</ul>
				</div>
			</div>
			<div class="game-info">
				<h2>Round: <span >{{turn}}</span></h2>
				<button class="start" :disabled="buttonFreezed"  @click="startMeth">Start</button>
				<p v-show="lose">Sorry, you lost after <span >{{rounds}}</span> rounds!</p>
			</div>
			<div class="game-options">
				<h2>Game Options:</h2>
				<input type="radio" name="mode" :disabled="buttonFreezed" :value="complexity"  v-model="complexity">1,5<br>
				<input type="radio" name="mode" :disabled="buttonFreezed" value="1000" v-model="complexity">1,0<br>
				<input type="radio" name="mode" :disabled="buttonFreezed" value="400" v-model="complexity">0,4<br>
			</div>
		<audio id="aud1" >
  <source src="./assets/sounds/1.ogg" type="audio/ogg">
  <source src="./assets/sounds/1.mp3" type="audio/mpeg">
</audio>
<audio id="aud2" >
  <source src="./assets/sounds/2.ogg" type="audio/ogg">
  <source src="./assets/sounds/2.mp3" type="audio/mpeg">
</audio>
<audio id="aud3" >
  <source src="./assets/sounds/3.ogg" type="audio/ogg">
  <source src="./assets/sounds/3.mp3" type="audio/mpeg">
</audio>
<audio id="aud4" >
	<source src="./assets/sounds/4.mp3" type="audio/mpeg">
  <source src="./assets/sounds/4.ogg" type="audio/ogg">
</audio>
	</div>
</template>

<script>

export default {
	data(){
		return{
			rounds: 0,
			buttonFreezed: false,
			lose: false,
			red: false,
			blue: false,
			yellow: false,
			green: false,
			complexity: 1500,
			order: [],
			playerOrder: [],
			flash: null,
			turn: 0,
			good: null,
			compTurn: null,
			interValid: null,
			noise: true 
		}
	},
	methods: {
		startMeth(){
			this.play()
		},
		play(){
			this.buttonFreezed = true
			this.order = []
			this.playerOrder = []
			this.flash = 0
			this.interValid = 0
			this.turn = 1
			this.rounds = 0
			this.lose = false
			this.good = true
			for(var i = 0; i < 100; i++){
				this.order.push(Math.floor(Math.random() * 4) + 1)
			}
			this.compTurn = true
			
			this.interValid = setInterval(this.gameTurn, this.complexity)
		},
		gameTurn() {
			if (this.flash == this.turn) {
				clearInterval(this.interValid)
				this.compTurn = false
				this.clearColor();
			}
			if (this.compTurn) {
				this.clearColor();
				setTimeout(() => {
					if (this.order[this.flash] == 1) this.one()
					if (this.order[this.flash] == 2) this.two()
					if (this.order[this.flash] == 3) this.three()
					if (this.order[this.flash] == 4) this.four()
					this.flash++
				}, 200)
			}


		},


		one(){
			if(this.noise) {
			let audio1 = document.querySelector('#aud1')
			audio1.play()
			}
			this.noise = true
			this.red = true
		},
		two(){
			if(this.noise) {
			let audio2 = document.querySelector('#aud2')
			audio2.play()
			}
			this.noise = true
			this.blue = true			
		},
		three(){
			if(this.noise) {
			let audio3 = document.querySelector('#aud3')
			audio3.play()
			}
			this.noise = true
			this.yellow = true
		},
		four(){
			if(this.noise) {
			let audio4 = document.querySelector('#aud4')
			audio4.play()
			}
			this.noise = true
			this.green = true
		},
		clearColor(){
			this.red = false
			this.blue = false
			this.yellow = false
			this.green = false
		},
		flashColor() {
			this.red = true
			this.blue = true
			this.yellow = true
			this.green = true
		},
		oneClick(){
			this.playerOrder.push(1)
			this.check()
			this.one()
			setTimeout(() => {
				this.clearColor()
			}, 300);
		},
		twoClick(){
			this.playerOrder.push(2)
			this.check()
			this.two()
			setTimeout(() => {
				this.clearColor()
			}, 300);
		},
		threeClick(){
			this.playerOrder.push(3)
			this.check()
			this.three()
			setTimeout(() => {
				this.clearColor()
			}, 300);
		},
		fourClick(){
			this.playerOrder.push(4)			
			this.check()
			this.four()
			setTimeout(() => {
				this.clearColor()
			}, 300);			
		},
		check(){
			if (this.playerOrder[this.playerOrder.length -1] !== this.order[this.playerOrder.length-1])
				this.good = false
				
			if (this.good == false) {
				this.flashColor();
				this.lose = true
				this.buttonFreezed = false
				this.turn = 0
				clearInterval(this.interValid)
				this.noise = false
			}
			if (this.turn == this.playerOrder.length && this.good) {
				this.turn++
				this.rounds = this.turn
				this.playerOrder = []
				this.compTurn = true
				this.flash = 0
				this.interValid = setInterval(this.gameTurn, this.complexity)
			}
			
		}
	},
	mounted(){
		console.log(this.complexity);
	}
}
</script>

<style >

</style>
