<!-- Create map here :) -->
<script>
	import '../../assets/global-styles.css';
	import { onMount } from 'svelte';
    import { csv, json } from 'd3-fetch';
	import WorldMap from "./lib/WorldMap.svelte";
    import CountryInfo from './lib/CountryInfo.svelte';
    
    let policyData = $state(new Map());
    let countriesList = $state([]);
    let mapData = $state([]);
    let selectedCountry = $state(null);

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
			mapData = await json("./data/countries_simplified.geojson");
		} catch (error) {
			console.error('Error loading CSV:', error);
		}
	}

	onMount(() => {
		loadPolicyData();
        loadMapData();
	});
</script>

<div class="layout">
    {#if mapData.features && policyData.size > 0}
        <div id="maparea">
            <div id="map">
                <WorldMap mapData={mapData} 
                bind:selectedCountry={selectedCountry} 
                countriesList={countriesList}/>
            </div>
            <div id="selector">
                <label for="country-select">Select a country:</label>
                <select id="country-select" 
                    name="country-select" 
                    bind:value={selectedCountry} 
                    onchange={() => console.log(selectedCountry)}>
                    {#each countriesList as entry}
                        <option style="font-family: OpenSans; font-size: 16px">{entry}</option>
                    {/each}
                </select>
            </div>
        </div>

        <div id="info">
            <CountryInfo 
                selectedCountry={selectedCountry} 
                policyData={policyData} />

        </div>
    {/if}
</div>

<style>
    #maparea { width: 100% }

    #map { width: 100% }

    #info { width: 100% }

    .layout {
        display: block; 
        gap: 1rem;      
        text-align: center;
    }

    label {
        font-family: TradeGothicBold;
        color: #0D534D;
    }

    select {
        font-family: OpenSans;
        font-size: 16px;
        border-radius: 5px;
        border: 1px solid grey;
        background-color: #dfdfdf;
    }
    
    @media (min-width: 1100px) {
        #maparea { width: 70% }
        #map { width: 100% }

        #info { width: 30% }

        .layout {
            display: flex;
            gap: 1rem;
            align-items: center;
            min-height: 100vh;
        }
    }
</style>