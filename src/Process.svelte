<script>
	export let name = "";
	export let xpos = 0;
	export let ypos = 0;
	export let w = 178;
	export let h = 128;
	export let telemetry = [];
	
	let blinker = 0
	
	$: {
		// just reference telemetry to trigger the change of blinked
		telemetry
		blinker = blinker + 1
		console.log("Blink:", name, blinker, xpos,ypos, w, h);
	}

	console.log("INIT",name,blinker, xpos,ypos, w, h);
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
  		from {fill:rgb(235, 237, 240);}
  		to {fill: rgb(64, 196, 99);}
	}
	@keyframes data-1-keyframes-2 {
  		from {fill:rgb(235, 237, 240);}
  		to {fill: rgb(64, 196, 99);}
	}

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
</style>

<svg>
	<g transform="translate({xpos + 16},{ypos})">
		<rect x={xpos} y={ypos} width={w} height={h} class="border processor">
			<title>{name}</title>
		</rect>
	</g>
	<g transform="translate({xpos},{ypos})">
		<g transform="translate({xpos},{ypos + 20})">
			<text x="20" y="-5" class="name">{name} ({xpos}:{ypos},{w}x{h})</text>
			{#each telemetry as t, i}
				<rect
					x="12"
					y={10 + i * 15}
					width="11"
					height="11"
					class="border data-{t.value === 0 ? "0" : "1-"+blinker%2}">
					<title>{t.value}</title>
					<!-- <animate attributeName="fill" values="0;5;0" dur="1s" repeatCount="indefinite" /> -->
				</rect>
				<text x="26" y={10 + i * 15 + 10} class="telemetry-name">
					{t.name}:

					<tspan class="telemetry-value">{t.value}</tspan>
				</text>
			{/each}
		</g>
	</g>
</svg>
