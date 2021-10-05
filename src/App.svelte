<script>
	import { onMount } from "svelte";
	import { Router, Route, Link } from "svelte-navigator";
	import AdminLogin from "./routes/AdminLogin.svelte";
	import AdminHome from "./routes/AdminHome.svelte";
	import WelcomeHome from "./routes/WelcomeHome.svelte";
	import StudentLogin from "./routes/StudentLogin.svelte";
	import LandingPage from "./routes/LandingPage.svelte";
</script>

<main>
	<div class="wrapper" />
	<Router primary={false}>
		<Route path="/*">
			<LandingPage />
		</Route>
		<Route path="/login">
			<WelcomeHome />
		</Route>
		<Route path="admin/*">
			<Route path="/">
				<AdminLogin />
			</Route>

			<Route path="/dash/*">
				<AdminHome />
			</Route>
		</Route>

		<Route path="student">
			<StudentLogin />
		</Route>
	</Router>
</main>

<style>
	main {
		max-width: 240px;
		margin: 0 auto;
	}

	.wrapper {
		max-width: 85em;
		margin: 2em auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
