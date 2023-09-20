<script lang="ts">
  import PlayField from './lib/PlayField.svelte';
  import SnakePiece from './lib/SnakePiece.svelte';
  interface Pos {
	x: number,
	y: number
  }
  enum Dir {
	Up,
	Down,
	Left,
	Right,
  }
  interface Snake {
	body: Pos[],
	direction: Dir
  }
  function randpos(): Pos {
	let aux: Pos = {x: 0, y: 0};
	aux.x = Math.round(Math.random()*16);
	aux.y = Math.round(Math.random()*16);
	return aux;
  }
  function moveSnake(s: Snake) {
	for(let i = s.body.length-1; i>0; i--){
	  s.body[i].x = s.body[i-1].x;
	  s.body[i].y = s.body[i-1].y;
	}
	switch(s.direction){
	case Dir.Up:
	  s.body[0].y--;
	  break;
	case Dir.Down:
	  s.body[0].y++;
	  break;
	case Dir.Left:
	  s.body[0].x--;
	  break;
	case Dir.Right:
	  s.body[0].x++;
	  break;  
	}
	s.body[0].x = s.body[0].x<0?s.body[0].x+16:s.body[0].x%16;
	s.body[0].y = s.body[0].y<0?s.body[0].y+16:s.body[0].y%16;
  }
  function checkAppleEaten(s: Snake, a: Pos): boolean {
	return (s.body[0].x == a.x && s.body[0].y == a.y);
  }
  let apple: Pos = randpos();
  let snake: Snake = {
	body: [
	  {x:0, y:0},
	  {x:0, y:0},
	],
	direction: Dir.Right
  };
 setInterval(()=>{
	moveSnake(snake);
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
