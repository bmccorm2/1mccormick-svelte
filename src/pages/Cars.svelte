<script lang="ts">
	import { location } from "../stores";
	import CarSource from "../components/CarSource.svelte";
	import CarChart from "../components/charts/CarChart.svelte";
	import { Row, Col, Spinner, Alert } from "sveltestrap";
	import { GET_CAR_DETAILS } from "../queries";
	import { queryStore, getContextClient } from "@urql/svelte";
	import { querystring } from "svelte-spa-router";

	const identifier = "carId";
	$: params = new URLSearchParams($querystring);
	$: carId = parseInt(params.get(identifier) || "");

	location.set(import.meta.env.VITE_ENVIRONMENT + "Cars");

	$: res = queryStore({
		client: getContextClient(),
		query: GET_CAR_DETAILS,
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
		<CarChart chartData={$res.data.carDetails} />
	</Row>
{/if}
