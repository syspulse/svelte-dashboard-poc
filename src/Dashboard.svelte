<script>
	import Process from "./Process.svelte";
	import Moveable from "svelte-moveable";

	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let blocks = [];
	// 	console.log(blocks);
	export let width = 2000;
	export let height = 1000;

	function handleRemoveBlock(event) {
		// console.log("RemoveBlock:", event.detail);
		dispatch('removeBlockApp', {
			name: event.detail.name
		});
		
	}
	function handleAddBlock(event) {
		// console.log("AddBlock:", event.detail);
		dispatch('addBlockApp', {
			block: event.detail.block
		});
	}

	function handleUpdateBlock(event) {
		// console.log("AddBlock:", event.detail);
		dispatch('updateBlockApp', {
			block: event.detail.block
		});
	}
</script>

<style>
	.dashboard {
		fill: rgb(235, 237, 240);
		stroke-width: 1;
		stroke: rgb(88, 96, 105);
	}
</style>

<!-- on:mousemove="{e => {	console.log(e);}}" -->
<svg {width} {height}>
	<g transform="translate(0,0)">
		<rect x="0" y="0" {width} {height} class="dashboard">
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
			telemetry={block.telemetry}
			on:addBlock={handleAddBlock}
			on:removeBlock={handleRemoveBlock}
			on:updateBlock={handleUpdateBlock} />
	{/each}
</svg>
