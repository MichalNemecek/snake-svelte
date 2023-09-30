<script lang="ts">
  export let x;
  export let y;
  const PW: number = 16;
</script>

<rect x={x*PW+1} y={y*PW+1} height={PW-2} width={PW-2} fill="red" />
