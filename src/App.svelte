<script lang="ts">
	import PlayField from './lib/PlayField.svelte';
	import SnakePiece from './lib/SnakePiece.svelte';
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
				{x:1, y:0},
				{x:0, y:0},
			];
		}
		move(){
			for(let i = this.body.length-1; i>0; i--){
				this.body[i] = this.body[i-1];
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
	}
	function randpos(): Vec2 {
	let aux: Vec2 = {x: 0, y: 0};
	aux.x = Math.round(Math.random()*16);
	aux.y = Math.round(Math.random()*16);
		return aux;
	}
	function checkAppleEaten(s: Snake, a: Vec2): boolean {
		return (s.body[0].x == a.x && s.body[0].y == a.y);
	}
	let apple: Vec2 = randpos();
	let snake: Snake = new Snake();
	setInterval(()=>{
		snake.move();
		snake = snake;
	},500);
</script>

<div class="w-100% text-center">
	<PlayField>
		{#each snake.body as sb}
			<SnakePiece x={sb.x} y={sb.y} />
		{/each}
	</PlayField>
	<button on:click={()=>{snake.direction=Dir.Down}}>Down</button>
</div>
