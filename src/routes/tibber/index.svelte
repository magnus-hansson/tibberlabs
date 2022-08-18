<script>
  import { onMount } from 'svelte'
  import item from './nodes.json'
  import consumptiondata from './consumption.json'
  import current from './current.json'

  import '@carbon/styles/css/styles.css'
  import '@carbon/charts/styles.css'
  import { BarChartSimple, LineChart } from '@carbon/charts-svelte'

  let consumption = []
  let currentPriceKrKW = 0
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
	}`

  const loadCurrent = async () => {
    const currentPrice =
      current.data.viewer.homes[0].currentSubscription.priceInfo.current.total
    currentPriceKrKW = currentPrice
  }

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

    consumption = await consumptiondata.consumption.nodes.map((x) => {
      return {
        group: 'Date',
        date: x.from,
        value: x.unitPrice,
        cost: x.cost,
        consumption: x.consumption
      }
    })

    await loadCurrent()
    //console.log(arrangedData);
    //console.log(nodes);
  })
</script>

<svelte:head>
  <title>Tibber!</title>
  <meta name="description" content="A tibber app" />
</svelte:head>

<div class="todos">
  <ul>
    <!-- 	{#each arrangedData as d}
			<li>{d.value}</li>
		{/each} -->
  </ul>

  <div><h1>Just nu {currentPriceKrKW} kr/kwH</h1></div>

  <LineChart
    data={consumption}
    options={{
      title: 'Kostnad per dag',
      height: '400px',
      axes: {
        left: { mapsTo: 'cost' },
        bottom: { mapsTo: 'date', scaleType: 'time' }
      }
    }}
  />
</div>
