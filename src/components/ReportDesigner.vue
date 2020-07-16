<template>
	<div>
		<h2>Stimulsoft Reports.JS Designer</h2>
		<div id="designer"></div>
	</div>
</template>

<script>
	export default {
		name: 'ReportDesigner',
		data() {
			return {
				loaded: 0
			};
		},
		head: {
			style: [
				{ type: 'text/css', src: 'stimulsoft/stimulsoft.designer.office2013.whiteblue.css' },
				{ type: 'text/css', src: 'stimulsoft/stimulsoft.viewer.office2013.whiteblue.css' }
			],
			script: [
				{ type: 'text/javascript', src: 'stimulsoft/stimulsoft.reports.js', id: 'stimulsoft_reports', async: false },
				{ type: 'text/javascript', src: 'stimulsoft/stimulsoft.viewer.js', id: 'stimulsoft_viewer', async: false },
				{ type: 'text/javascript', src: 'stimulsoft/stimulsoft.designer.js', id: 'stimulsoft_designer', async: false }
			]
		},
		mounted: function () {
			this.$on('okHead', function () {
				const sr = document.getElementById('stimulsoft_reports');
				sr.onload = this.stiInit;
				const sv = document.getElementById('stimulsoft_viewer');
				sv.onload = this.stiInit;
				const sd = document.getElementById('stimulsoft_designer');
				sd.onload = this.stiInit;
			});
		},
		methods: {
			onBeginProcessData(e) {
				e.headers.push({ key: "Authorization", value: "Bearer 123456789" });
				console.log(e);
			},
			stiInit() {
				console.log(this.loaded);
				if (this.loaded < 3) {
					this.loaded++;
					return;
				}
				console.log("Loading Designer view");

				console.log("Set full screen mode for the designer");
				var options = new window.Stimulsoft.Designer.StiDesignerOptions();
				options.appearance.fullScreenMode = false;

				console.log("Create the report designer with specified options");
				var designer = new window.Stimulsoft.Designer.StiDesigner(
					options,
					"StiDesigner",
					false
				);

				designer.onBeginProcessData = this.onBeginProcessData;

				console.log("Create a new report instance");
				var report = new window.Stimulsoft.Report.StiReport();

				console.log("Load report from url");
				report.loadFile("reports/SimpleList.mrt");

				// https://services.odata.org/TripPinRESTierService/(S(vuzbl01wsrehumusahsgyqbl))/
				// https://services.odata.org/TripPinRESTierService/(S(vuzbl01wsrehumusahsgyqbl))/People('scottketchum')/Trips
				var db = new window.Stimulsoft.Report.Dictionary.StiODataDatabase(
					"OpenData",
					"odata",
					"https://services.odata.org/TripPinRESTierService/(S(vuzbl01wsrehumusahsgyqbl))/",
					false
				);
				report.dictionary.databases.add(db);

				report.onBeginProcessData = this.onBeginProcessData;

				console.log("Edit report template in the designer");
				designer.report = report;

				console.log("Rendering the viewer to selected element");
				designer.renderHtml("designer");

				console.log("Loading completed successfully!");
			}
		}
	}
</script>

<style>
	h2 {
		margin-block-start: 0.2em;
		margin-block-end: 0.2em;
	}
</style>
