<script lang="ts">
  import dayjs from "dayjs";
  import { Card, CardHeader, CardBody, Table } from "sveltestrap";
  interface row {
    date: string;
    miles_per_gallon: number;
    price_per_gallon: number;
    notes: string;
  }
  export let tableRows: [row];
</script>

<Card>
  <CardHeader class="text-center fs-4">Mileage Data</CardHeader>
  <CardBody>
    <Table size="md" hover responsive>
      <thead>
        <tr>
          <th>Date</th>
          <th>Miles/Gallon</th>
          <th>Price/Gallon</th>
          <th>Notes</th>
        </tr>
      </thead>
      <tbody>
        {#each tableRows as row}
          <tr>
            <td>{dayjs(row.date).format("YYYY-MM-DD")}</td>
            <td>{row.miles_per_gallon.toFixed(2)}</td>
            <td>${row.price_per_gallon.toFixed(2)}</td>
            <td>{row.notes === null ? "" : row.notes}</td>
          </tr>
        {/each}
      </tbody>
    </Table>
  </CardBody>
</Card>
