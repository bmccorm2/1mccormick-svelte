<script lang="ts">
  import { Col } from "sveltestrap";
  import { location } from "svelte-spa-router";

  interface cars {
    id: number;
    displayName: string;
  }

  export let cars: [cars];
</script>

<Col>
  <div class="text-center mb-2">
    {#each cars as car}
      <a href={"#" + $location + "?carId=" + car.id} class="p-2"
        >{car.displayName}</a
      >
    {/each}
  </div>
</Col>
