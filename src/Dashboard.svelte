<script>
	import Process from "./Process.svelte";
	import Moveable from "svelte-moveable";

    const frame = {
        translate: [0, 0],
    };
	let target;
	
	export let blocks = [];
	// 	console.log(blocks);
	export let width = 2000;
	export let height = 1000;
</script>

<style>
	.dashboard {
		fill: rgb(235, 237, 240);
		stroke-width: 1;
		stroke: rgb(88, 96, 105);
	}
</style>

<!-- on:mousemove="{e => {	console.log(e);}}" -->
<svg width="{width}" height="{height}"
	
>
	<g transform="translate(0,0)">
		<rect x="0" y="0" width="{width}" height="{height}" class="dashboard">
			<title>dashboard</title>
		</rect>
	</g>

	{#each blocks as block, i}
		<Process
			name={block.name}
			icon={block.icon}
			xpos={block.x}
			ypos={block.y}
			h={block.h}
			telemetry={block.telemetry} />
	{/each}
</svg>
