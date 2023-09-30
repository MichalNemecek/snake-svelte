<script lang="ts">
	import PlayField from './lib/PlayField.svelte';
	import SnakePiece from './lib/SnakePiece.svelte';
	import Apple from './lib/Apple.svelte';
	interface Vec2 {
	  x: number,
	  y: number
	}
	enum Dir {
	  Up,
	  Down,
	  Left,
	  Right,
	}
	class Snake {
	  body: Vec2[];
	  direction: Dir;
	  constructor(){
		this.direction = Dir.Right;
		this.body = [
		  {x:0, y:0},
		  {x:0, y:0},
		];
	  }
	  move(){
		for(let i = this.body.length-1; i>0; i--){
		  this.body[i] = {...this.body[i-1]};
		}
		switch(this.direction){
		case Dir.Left:
		  this.body[0].x--;
		  break;
		case Dir.Right:
		  this.body[0].x++;
		  break;
		case Dir.Up:
		  this.body[0].y--;
		  break;
		case Dir.Down:
		  this.body[0].y++;
		  break;
		}
		this.body[0].x = this.body[0].x<0?this.body[0].x+16:this.body[0].x%16;
		this.body[0].y = this.body[0].y<0?this.body[0].y+16:this.body[0].y%16;
	  }
	  lengthen(){
		this.body.push(this.body[this.body.length-1]);
	  }
	  checkAppleEaten(a: Vec2): boolean {
		return (this.body[0].x == a.x && this.body[0].y == a.y);
	  }
	  checkOccupiedByMe(a: Vec2): boolean {
		for(let i = 1; i<this.body.length; i++){
		  if(a.x == this.body[i].x && a.y == this.body[i].y){
			return true;
		  }
		}
		return false;
	  }
	  checkSelfCollision(): boolean {
		for(let i = 1; i<this.body.length; i++){
		  if(this.body[0].x == this.body[i].x && this.body[0].y == this.body[i].y){
			return true;
		  }
		}
		return false;
	  }
	}
	let snake: Snake = new Snake();
	function randpos(): Vec2 {
	  let aux: Vec2 = {x: 0, y: 0};
	  do {
	  	aux.x = Math.floor(Math.random()*16);
		aux.y = Math.floor(Math.random()*16);
	  }while(snake.checkOccupiedByMe(aux));
	  return aux;
	}
	
	let apple: Vec2 = randpos();
	let gameover: boolean = false;
	let interval: number = setInterval(()=>{
	  snake.move();
	  if(snake.checkAppleEaten(apple)){
		if(snake.body.length == 256){
		  console.log("Snake fills whole board!");
		  gameover = true;
		  clearInterval(interval);
		  return;
		}
		apple = randpos();
		snake.lengthen();
	  }
	  if(snake.checkSelfCollision()){
		console.log("Collided!");
		gameover = true;
		clearInterval(interval);
		return;
	  }
	  snake = snake;
	},200);
	function onKeyDown(e: KeyboardEvent){
	  switch(e.keyCode) {
	  case 38:
		if(snake.body[0].y-1 != snake.body[1].y){
		  snake.direction = Dir.Up;
		}
		break;
	  case 40:
		if(snake.body[0].y+1 != snake.body[1].y){
		  snake.direction = Dir.Down;
		}
		break;
	  case 37:
		if(snake.body[0].x-1 != snake.body[1].x){
		  snake.direction = Dir.Left;
		}
		break;
	  case 39:
		if(snake.body[0].x+1 != snake.body[1].x){
		  snake.direction = Dir.Right;
		}
		break;
	  }
	}
</script>

<div class="w-100% text-center">
	<PlayField>
		{#each snake.body as sb}
		  <SnakePiece x={sb.x} y={sb.y} />
		{/each}
		<Apple x={apple.x} y={apple.y} />
	</PlayField>
	<h3>{gameover?("Game Over! Score: "+(snake.body.length-2)+"00"):""}</h3>
</div>
<svelte:window on:keydown|preventDefault={onKeyDown} />
