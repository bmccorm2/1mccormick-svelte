<script lang="ts">
	import YieldChart from "../components/charts/YieldChart.svelte";
	import { Row, Col, Spinner, Alert } from "sveltestrap";
	import { queryStore, getContextClient } from "@urql/svelte";
	import { GET_YIELDS } from "../queries";
	import { location } from "../stores";

	location.set(import.meta.env.VITE_ENVIRONMENT + "Yields");

	const res = queryStore({
		client: getContextClient(),
		query: GET_YIELDS,
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
{:else if $res.data.yields && $res.data.yields.length === 3}
	<Row>
		<Col>
			<YieldChart chartData={$res.data.yields} />
		</Col>
	</Row>
{:else}
	<Row>
		<Col class="text-center">
			<Alert color="danger">Data Not Available</Alert>
		</Col>
	</Row>
{/if}
