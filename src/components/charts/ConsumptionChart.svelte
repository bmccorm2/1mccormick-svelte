<script lang="ts">
  import dayjs from "dayjs";
  import { afterUpdate } from "svelte";
  import Chart from "chart.js/auto";
  interface chartData {
    date: string;
    miles_per_gallon: number;
    price_per_gallon: number;
  }

  export let chartData: [chartData];
  let ctx: any;
  let myChart: Chart;

  const options: any = {
    responsive: true,
    interaction: {
      mode: "index",
      intersect: false,
    },
    plugins: {
      legend: {
        labels: {
          color: "white",
        },
      },
    },
    scales: {
      right: {
        type: "linear",
        position: "right",
        grid: {
          drawOnChartArea: false,
        },
      },
      left: {
        type: "linear",
        position: "left",
      },
    },
  };

  $: data = {
    labels: chartData.map((o) => dayjs(o.date).format("YYYY-MM-DD")).reverse(),
    datasets: [
      {
        label: "Miles Per Gallon",
        data: chartData
          .map((o) => parseFloat(o.miles_per_gallon.toFixed(2)))
          .reverse(),
        yAxisID: "right",
        backgroundColor: "blue",
        borderColor: "lightblue",
        fill: false,
        radius: 7,
      },
      {
        label: "Price Per Gallon",
        data: chartData
          .map((o) => parseFloat(o.price_per_gallon.toFixed(2)))
          .reverse(),
        yAxisID: "left",
        backgroundColor: "green",
        borderColor: "lightgreen",
        fill: false,
        radius: 7,
      },
    ],
  };

  afterUpdate(() => {
    if (myChart) myChart.destroy();
    myChart = new Chart(ctx, {
      type: "line",
      data,
      options,
    });
  });
</script>

<canvas id="consumptionChart" width="2" height="1" bind:this={ctx} />
