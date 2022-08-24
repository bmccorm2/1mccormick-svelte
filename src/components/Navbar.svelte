<script lang="ts">
  import active from "svelte-spa-router/active";
  import { Nav, Row, Col, Navbar } from "sveltestrap";
  import { location } from "../stores";

  let locationValue: string;
  location.subscribe((value) => {
    locationValue = value;
  });

  const activeCarPath = new RegExp("/cars*");
  const activeConsumptionPath = new RegExp("/consumption*");
</script>

<Row>
  <Col md="5" class="text-center">
    <h1>{locationValue}</h1>
  </Col>
  <Col md="7">
    <Navbar>
      <Nav pills>
        <a class="nav-link" href="/#/" use:active={"/"}>Home</a>
        <a
          class="nav-link"
          href="/#/consumption?carId=1"
          use:active={activeConsumptionPath}>Consumption</a
        >
        <a class="nav-link" href="/#/yield" use:active={"/yield*"}>Yield</a>
        <a class="nav-link" href="/#/cars?carId=3" use:active={activeCarPath}
          >Cars</a
        >
        <a class="nav-link" href="/#/snake" use:active={"/snake"}>Snake</a>
        <a class="nav-link" href="/#/life" use:active={"/life"}>Life</a>
      </Nav>
    </Navbar>
  </Col>
</Row>
