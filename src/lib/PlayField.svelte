<script lang="ts">
	const PW: number = 16;
</script>

<svg width={PW*16} height={PW*16} class="mx-auto mt-3">
	<rect x="0" y="0" height={PW*16} width={PW*16} fill="black"/>
	<slot />
</svg>
