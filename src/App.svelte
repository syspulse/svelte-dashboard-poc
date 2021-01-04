<script>
	import { onMount } from "svelte";
	import Process from './Process.svelte';
	import Icon, {iconNames} from "./Icon.svelte"
	import Dashboard from "./Dashboard.svelte";

	console.log("icon",iconNames[getRand(iconNames.length)+1])
	let time = new Date();
	let tick = 1;

	function getRand(max) {
		return Math.abs(Math.floor(Math.random() * Math.floor(max)));
	}

	// $: console.log(tick)

	let initialBlocks = Array(10).fill().map(
		(_,i) => (
		{
			x: getRand(850),
			y: getRand(450),
			h: 128,
			name: "Source-"+i,
			icon: iconNames[getRand(iconNames.length)+1],
			telemetry: [
				{ name: "tx-in", value: 0 },
				{ name: "tx-out", value: 0 },
				{ name: "total", value: 0 },
			],
		})
	);

	let blocks = initialBlocks

	$: blocks = blocks.map(
		(block,i) => (
			{
			x: block.x,
			y: block.y,
			h: block.h,
			name: block.name,
			icon: block.icon,
			telemetry: [
				{ name: block.telemetry[0].name, value: getRand(tick + 100) },
				{ name: block.telemetry[1].name, value: getRand(0) },
				{ name: block.telemetry[2].name, value: getRand(2) },
			],
		})
	);

	function handleRemoveBlock(event) {
		console.log("RemoveBlock:", event.detail,event.detail.name);
		blocks = blocks.filter((b) => b.name !== event.detail.name);
	}

	function handleAddBlock(event) {
		console.log("AddBlock:", event.detail, event.detail.block);
		let block = event.detail.block
		blocks = blocks.concat({
			x: block.x,
			y: block.y,
			h: block.h,
			name: block.name,
			icon: block.icon,
			telemetry: block.telemetry,

		});
	}

	function handleUpdateBlock(event) {
		console.log("UpdateBlock:", event.detail, event.detail.block);
	
		let block1 = event.detail.block
		let i = blocks.findIndex( (b) => b.name === block1.name)
		let block0 = blocks[i]
		
		blocks[i] = block1
		console.log("UpdateBlock:", i, blocks);

		// const block = event.detail.block
		// let blocks1 = blocks.filter((b) => b.name !== block.name);
		// blocks1 = blocks1.concat({
		// 	x: block.x,
		// 	y: block.y,
		// 	h: block.h,
		// 	name: block.name,
		// 	icon: block.icon,
		// 	telemetry: block.telemetry,

		// });
		// blocks = blocks1;
	}

	onMount(() => {
		const interval = setInterval(() => {
			//time = new Date();
			tick = tick + 1;
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});
</script>

<h1>Telemetry: {blocks.length}</h1>
<!-- <Icon name="arrow"/> -->
<!-- <Icon name="coffee"/> -->
<Dashboard 
	{blocks} 
	on:addBlockApp={handleAddBlock}
	on:removeBlockApp={handleRemoveBlock}
	on:updateBlockApp={handleUpdateBlock}
/>
