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

	// blocks = [
	// 	{
	// 		x: 0,
	// 		y: 5,
	// 		name: "Source",
	// 		telemetry: [
	// 			{ name: "tx-in", value: getRand(tick + 100) },
	// 			{ name: "tx-out", value: getRand(0) },
	// 			{ name: "total", value: getRand(tick + 10) },
	// 		],
	// 	},
	// 	{
	// 		x: 75,
	// 		y: 18,
	// 		h: 80,
	// 		name: "Processor",
	// 		telemetry: [
	// 			{ name: "db-w", value: getRand(tick + 1000) },
	// 			{ name: "db-r", value: getRand(tick + 1000) },
	// 		],
	// 	},
	// 	{
	// 		x: 150,
	// 		y: 5,
	// 		name: "Sink",
	// 		telemetry: [
	// 			{ name: "input-1", value: getRand(10) },
	// 			{ name: "input-2", value: getRand(20) },
	// 			{ name: "input-3", value: 0 },
	// 			{ name: "input-4", value: 0 },
	// 		],
	// 	},
	// ];
	let initialBlocks = Array(1).fill().map(
		(_,i) => (
		{
			x: getRand(300),//getRand(850),
			y: getRand(200),//getRand(450),
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

	// onMount(() => {
	// 	const interval = setInterval(() => {
	// 		//time = new Date();
	// 		tick = tick + 1;
	// 	}, 1000);

	// 	return () => {
	// 		clearInterval(interval);
	// 	};
	// });
</script>

<h1>Telemetry: {blocks.length}</h1>
<Icon name="arrow"/>
<Icon name="coffee"/>
<Dashboard {blocks} />
