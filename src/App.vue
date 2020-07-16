<template>
	<div id="app">
		<ReportDesigner></ReportDesigner>
	</div>
</template>

<script>
	import ReportDesigner from './components/ReportDesigner.vue';
	export default {
		name: 'App',
		components: {
			ReportDesigner: ReportDesigner
		}
	}
</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
	}
</style>
