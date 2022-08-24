<script lang="ts">
	import { onMount } from "svelte";
	import Chart from "chart.js/auto";
	import dayjs from "dayjs";
	import { CardHeader, CardBody, Card } from "sveltestrap";
	import humanize from "humanize-plus";

	interface chartData {
		x: number;
		y: number;
		price: number;
		miles: number;
		url: string;
		displayName: string;
		year: number;
		effectiveDate: string;
		distance: number;
	}

	export let chartData: Array<chartData>;
	let ctx: any;
	let myChart: Chart;
	let price: number;
	let url: string;
	let displayName: string;
	let year: number;
	let miles: number;
	let distance: number;
	let effectiveDate: string;
	let isSelected = false;
	const colors = ["maroon", "blue", "green", "orange", "purple", "pink"];
	const years = [...new Set(chartData.map((o) => o.year))];

	const options = {
		plugins: {
			legend: {
				labels: {
					color: "white",
				},
			},
		},
	};

	$: data = {
		datasets: years.map((year, index) => {
			return {
				label: year.toString(),
				backgroundColor: colors[index],
				data: chartData
					.filter((o) => o.year === year)
					.map((o) => {
						return {
							x: o.price,
							y: o.miles,
							price: o.price,
							miles: o.miles,
							url: o.url,
							displayName: o.displayName,
							year: o.year,
							effectiveDate: dayjs(o.effectiveDate).format("YYYY-MM-DD"),
							distance: o.distance,
						};
					}),
				radius: 8,
			};
		}),
	};

	onMount(() => {
		myChart = new Chart(ctx, {
			type: "scatter",
			data,
			options,
		});
	});

	const handleClick = (e: MouseEvent) => {
		const points = myChart.getElementsAtEventForMode(e, "nearest", { intersect: true }, true);
		if (!points.length) return;
		isSelected = true;
		const firstPoint = points[0];
		const value: any = myChart.data.datasets[firstPoint.datasetIndex].data[firstPoint.index];
		({ price, miles, url, displayName, year, effectiveDate } = value);
	};
</script>

<div class="col-12" class:smallChart={isSelected}>
	<canvas id="myChart" width="2" height="1" bind:this={ctx} on:click={handleClick} />
</div>
<div class:hideDetails={!isSelected} class="col-3">
	<Card>
		<CardHeader class="text-center fs-4">Details</CardHeader>
		<CardBody>
			<p class="fw-bolder">
				<a href={url} target="_blank" rel="noopener noreferrer">
					{displayName}
				</a>
			</p>
			<p class="bold">
				<small>Price:</small>&nbsp;
				{price ? "$" + humanize.intComma(price) : ""}
			</p>
			<p class="bold">
				<small>Miles:</small>&nbsp;
				{miles ? humanize.intComma(miles) : ""}
			</p>
			<p class="bold">
				<small>Year:</small>
				{year}
			</p>
			<p class="bold">
				<small>Distance (mi):</small>&nbsp;
				{distance ? humanize.intComma(distance) : ""}
			</p>
			<p class="bold">
				<small>Last Updated:</small>&nbsp;
				{effectiveDate}
			</p>
		</CardBody>
	</Card>
</div>

<style>
	.hideDetails {
		display: none;
	}
	.smallChart {
		width: 75%;
	}
	.bold {
		font-weight: bolder;
	}
</style>
