<script>
	import { onMount } from 'svelte';
	import item from './nodes.json';
	import consumptiondata from './consumption.json';

	import '@carbon/styles/css/styles.css';
	import '@carbon/charts/styles.css';
	import { BarChartSimple, HistogramChart } from '@carbon/charts-svelte';

	
	let kaj = [];
	
	let query = `{
		viewer {
			homes {
			consumption(resolution: HOURLY, last: 10) {
				nodes {
				from
				to
				cost
				unitPrice
				unitPriceVAT
				consumption
				consumptionUnit
				}
			}
			}
		}
	}`;

	onMount(async () => {
		/* const res = await fetch('https://api.tibber.com/v1-beta/gql', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				Authorization: 'Bearer K0GseNMSHm7EwtCVv29z0vawzqaEmdoT263DV2xIL3Ax'
			},
			body: JSON.stringify({ query })
		});
		let jsonres = await res.json();
		console.log(jsonres.data.viewer.homes[0].consumption.nodes);
		nodes = jsonres.data.viewer.homes[0].consumption.nodes;    */
		//nodes = item.nodes;
		/* arrangedData = nodes.map((x) => {
			return { group: x.from, value: x.unitPrice };
		}); */

		kaj = await consumptiondata.consumption.nodes.map((x) => {
			return {
				group: x.from,
				date: x.from,
				value: x.unitPrice,
				cost: x.cost,
				consumption: x.consumption
			};
		});
		console.log(kaj);
		//console.log(arrangedData);
		//console.log(nodes);
	});
</script>

<svelte:head>
	<title>Todos</title>
	<meta name="description" content="A tibber app" />
</svelte:head>

<div class="todos">
	<ul>
		<!-- 	{#each arrangedData as d}
			<li>{d.value}</li>
		{/each} -->
	</ul>

	<BarChartSimple
		data={kaj}
		options={{
			title: 'Tibber',
			height: '600px',
			axes: {
				left: { mapsTo: 'cost' },
				bottom: { mapsTo: 'date', scaleType: 'labels' }
			}
		}}
	/>

	<!-- <BarChartSimple
		data={[
			{ group: 'Qty', value: 65000 },
			{ group: 'More', value: 29123 },
			{ group: 'Sold', value: 35213 },
			{ group: 'Restocking', value: 51213 },
			{ group: 'Misc', value: 16932 }
		]}
		options={{
			title: 'Simple bar (discrete)',
			height: '400px',
			axes: {
				left: { mapsTo: 'value' },
				bottom: { mapsTo: 'group', scaleType: 'labels' }
			}
		}}
	/> -->
</div>
