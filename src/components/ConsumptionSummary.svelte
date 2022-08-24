<script lang="ts">
	import humanize from "humanize-plus";
	import { Card, CardHeader, CardBody, Alert } from "sveltestrap";

	interface summary {
		total_miles: number;
		total_price: number;
		total_miles_per_gallon: number;
		total_price_per_gallon: number;
	}

	export let summary: summary;
</script>

<Card>
	<CardHeader class="text-center fs-4">Summary</CardHeader>
	<CardBody class="text-center">
		<Alert color="success">
			<small>Total Miles: &nbsp;</small>
			<b>{humanize.formatNumber(summary.total_miles, 0)}</b>
		</Alert>
		<Alert color="success">
			<small>Total Cost: &nbsp;</small>
			<b>${humanize.formatNumber(summary.total_price, 2)}</b>
		</Alert>
		<Alert color="success">
			<small>Total Miles Per Gallon: &nbsp;</small>
			<b>{humanize.formatNumber(summary.total_miles_per_gallon, 2)}</b>
		</Alert>
		<Alert color="success">
			<small>Total Cost Per Gallon: &nbsp;</small>
			<b>${humanize.formatNumber(summary.total_price_per_gallon, 2)}</b>
		</Alert>
	</CardBody>
</Card>
