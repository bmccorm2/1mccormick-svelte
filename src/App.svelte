<script lang="ts">
	import "bootswatch/dist/darkly/bootstrap.min.css";
	import Home from "./pages/Home.svelte";
	import Consumption from "./pages/Consumption.svelte";
	import Snake from "./pages/Snake.svelte";
	import Cars from "./pages/Cars.svelte";
	import Yield from "./pages/Yield.svelte";
	import Navbar from "./components/Navbar.svelte";
	import Router from "svelte-spa-router";
	import { createClient, setContextClient } from "@urql/svelte";
	import { Container } from "sveltestrap";
	import Life from "./pages/Life.svelte";

	const client = createClient({
		url: import.meta.env.VITE_GRAPHQL_URI,
	});
	setContextClient(client);

	const routes = {
		"/": Home,
		"/consumption": Consumption,
		"/yield": Yield,
		"/cars": Cars,
		"/snake": Snake,
		"/life": Life,
	};
	let title = import.meta.env.VITE_TITLE;
</script>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<Container md class="p-3">
	<Navbar />
	<hr />
	<Router {routes} />
</Container>
