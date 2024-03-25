<template>
	<div id="app">
		<TheForm />
	</div>
</template>

<script>
import TheForm from "./components/TheForm.vue"
export default {
	name: "App",
	components: {
		TheForm,
	},
}
</script>

<style>
html {
	font-size: 62.5%;
	font-family: "Montserrat", sans-serif;
}
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
.error {
	border-color: red;
}
.error-message {
	color: red;
}
.success-message {
	color: green;
	font-weight: bold;
}
</style>
