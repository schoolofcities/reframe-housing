<!-- Create map here :) -->
<script>
	import { onMount } from 'svelte';
    import { csv, json } from 'd3-fetch';
	import WorldMap from "./lib/WorldMap.svelte";
    
    let policyData = $state(new Map());
    let countriesList = $state([]);
    let mapData = $state([]);
    let selectedCountry = $state("");

    async function loadPolicyData() {
		try {
			let rawPolicyData = await csv("./data/country-policies.csv");
            rawPolicyData.forEach((country) => {
                policyData.set(country.country, country);
            })
            countriesList = [...policyData.keys()];
		} catch (error) {
			console.error('Error loading CSV:', error);
		}
	}

    async function loadMapData() {
		try {
			mapData = await json("./data/countries.geojson");
            console.log(mapData.features.length);
		} catch (error) {
			console.error('Error loading CSV:', error);
		}
	}

	onMount(() => {
		loadPolicyData();
        loadMapData();
	});
</script>

<div>
    {#if mapData.features && policyData.size > 0}
        <WorldMap mapData={mapData} bind:selectedCountry={selectedCountry} countriesList={countriesList}></WorldMap>
        
        <label for="country-select">Select a country:</label>
        <select id="country-select" 
            name="country-select" 
            bind:value={selectedCountry} 
            onchange={() => console.log(selectedCountry)}>
            {#each countriesList as entry}
                <option>{entry}</option>
            {/each}
        </select>
    {/if}
</div>