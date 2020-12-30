<script>
	import Icon, { directions, iconNames } from "./Icon.svelte";
	// import Moveable from "svelte-moveable";
	import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	export let name = "";
	export let xpos = 0;
	export let ypos = 0;
	export let w = 188;
	export let h = 128;
	export let telemetry = [];
	export let icon = "arrow";

	let moving = false;
	let movePos = { x: 0, y: 0 };
	let frame = {
		translate: [0, 0],
	};
	let target;

	let blinker = 0;

	$: {
		// just reference telemetry to trigger the change of blinked
		telemetry;
		blinker = blinker + 1;
		//console.log("Blink:", name, blinker, xpos,ypos, w, h);
	}

	function removeBlock(block) {
		dispatch('removeBlock', {
			name: block
		});
	}

	function addBlock(block) {
		dispatch('addBlock', {
			block: block
		});
	}

	function updateBlock(block) {
		dispatch('updateBlock', {
			block: block
		});
	}

	console.log("INIT", name, blinker, xpos, ypos, w, h);
</script>

<style>
	.processor {
		fill: rgb(235, 237, 240);
	}

	.data-0 {
		fill: rgb(235, 237, 240);
	}
	.data-1-0 {
		fill: rgb(64, 196, 99);
		animation: data-1-keyframes-1 0.5s 1;
	}
	.data-1-1 {
		fill: rgb(64, 196, 99);
		animation: data-1-keyframes-2 0.5s 1;
	}

	@keyframes data-1-keyframes-1 {
		from {
			fill: rgb(235, 237, 240);
		}
		to {
			fill: rgb(64, 196, 99);
		}
	}
	@keyframes data-1-keyframes-2 {
		from {
			fill: rgb(235, 237, 240);
		}
		to {
			fill: rgb(64, 196, 99);
		}
	}

	.debug,
	.text,
	.name,
	.telemetry-name,
	.telemetry-value {
		fill: rgb(106, 101, 116);
		display: block;
		font-size: 13px;
		line-height: 15px;
		font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Helvetica,
			Arial, sans-serif, Apple Color Emoji, Segoe UI Emoji;
		text-align: left;
		white-space: nowrap;
	}

	.debug {
		font-size: 9px;
	}

	.telemetry-name {
		fill: rgb(106, 101, 116);
	}
	.telemetry-value {
		fill: black;
	}

	.border {
		/* fill:rgb(${color}); */
		stroke-width: 1;
		stroke: rgb(88, 96, 105);
	}

	.border-moving {
		stroke-width: 2;
		stroke: lightblue;
	}

	.icon {
		font-size: 2em;
	}

	.draggable {
  		cursor: move;
	}
</style>

<!-- <Moveable
	draggable={true}
	{target}
	throttleDrag={0}
	on:dragStart={({ detail: { set , clientX, clientY} }) => {
		set(frame.translate);
		
		moving = true
		movePos.x = clientX
		movePos.y = clientY
		
		console.log('onDragStart', frame.translate);
	}}
	on:drag={({ detail: { target, beforeTranslate, clientX, clientY } }) => {
		console.log('onDrag', clientX, clientY);
		frame.translate = beforeTranslate;
		target.style.transform = `translate(${beforeTranslate[0]}px, ${beforeTranslate[1]}px)`;
		xpos = xpos + (clientX - movePos.x)
		ypos = ypos + (clientY - movePos.y)
		movePos.x = clientX
		movePos.y = clientY
	}}
	on:dragEnd={({ detail: { target, isDrag, clientX, clientY } }) => {
		console.log('onDragEnd', clientX, clientY, movePos, target, isDrag);
		moving = false
		xpos = xpos + (clientX - movePos.x)
		ypos = ypos + (clientY - movePos.y)
		
	}} /> -->

<svg class="target" bind:this={target} x={xpos} y={ypos}
	on:mousemove="{e => {
		if(moving) {
			xpos = xpos + (e.clientX - movePos.x)
			ypos = ypos + (e.clientY - movePos.y)
			movePos.x = e.clientX
			movePos.y = e.clientY

			updateBlock({
				x: xpos,
				y: ypos,
				w: w,
				h: h,
				name: name,
				icon: icon,
				telemetry: telemetry
			})
		}
		
	}}"
	on:mousedown="{e => {
		
		movePos.x = e.clientX
		movePos.y = e.clientY
		moving = true

		updateBlock({
				x: xpos,
				y: ypos,
				w: w,
				h: h,
				name: name,
				icon: icon,
				telemetry: telemetry
			})
	}}"
	
	on:mouseup="{ e=> {
		moving = false;
		
	}}"

>
	<g transform="translate(16,0)">
		<rect x="0" y="0" width={w} height={h} class={(moving ? "border-moving": "border")+" processor"}>
			<title>{name}</title>
		</rect>

		<g transform="translate(-16,20)">
			<text x="20" y="-5" class="name">
				{name}
				<tspan class="debug">({xpos}:{ypos},{w}x{h})</tspan>
			</text>
			<Icon name={icon} xpos={w - 15} ypos="-15" />
			{#each telemetry as t, i}
				<rect
					x="12"
					y={10 + i * 15}
					width="11"
					height="11"
					class="border data-{t.value === 0 ? '0' : '1-' + (blinker % 2)}">
					<title>{t.value}</title>
				</rect>
				<text x="26" y={10 + i * 15 + 10} class="telemetry-name">
					{t.name}:

					<tspan class="telemetry-value">{t.value}</tspan>
				</text>
			{/each}
		</g>
	</g>
</svg>
