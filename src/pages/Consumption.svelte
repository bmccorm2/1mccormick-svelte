<script lang="ts">
	import CarSource from "../components/CarSource.svelte";
	import Input from "../components/Input.svelte";
	import MileageData from "../components/MileageData.svelte";
	import ConsumptionSummary from "../components/ConsumptionSummary.svelte";
	import ConsumptionChart from "../components/charts/ConsumptionChart.svelte";
	import { queryStore, getContextClient } from "@urql/svelte";
	import { GET_CONSUMPTION } from "../queries";
	import { Row, Col, Spinner, Alert } from "sveltestrap";
	import { location } from "../stores";
	import { querystring } from "svelte-spa-router";

	location.set(import.meta.env.VITE_ENVIRONMENT + "Consumption");
	const identifier = "carId";
	$: params = new URLSearchParams($querystring);
	$: carId = parseInt(params.get(identifier) || "");

	$: res = queryStore({
		client: getContextClient(),
		query: GET_CONSUMPTION,
		variables: { carId },
	});
</script>

{#if $res.fetching}
	<Row>
		<Col class="text-center">
			<Spinner color="success" />
		</Col>
	</Row>
{:else if $res.error}
	<Row>
		<Col class="text-center">
			<Alert color="danger">Service Down</Alert>
		</Col>
	</Row>
{:else}
	<Row><CarSource cars={$res.data.cars} /></Row>
	<Row>
		<Col md="3"><Input {carId} /></Col>
		<Col md="6">
			<MileageData tableRows={$res.data.consumption.slice(0, 5)} />
		</Col>
		<Col md="3">
			<ConsumptionSummary summary={$res.data.summary} />
		</Col>
	</Row>
	<Row>
		<ConsumptionChart chartData={$res.data.consumption} />
	</Row>
{/if}
