<!-- Create map here :) -->
<script>
	import '../../assets/global-styles.css';
	import { onMount } from 'svelte';
    import { csv, json } from 'd3-fetch';
	import WorldMap from "./lib/WorldMap.svelte";
    import CountryInfo from './lib/CountryInfo.svelte';
    import Select from 'svelte-select';

    let policyData = $state(new Map());
    let countriesList = $state([]);
    let mapData = $state([]);
    let selectedCountry = $state("Canada");
    let selectItems = [];

    async function loadPolicyData() {
		try {
			let rawPolicyData = await csv("./data/country-policies.csv");
            rawPolicyData.forEach((country) => {
                policyData.set(country.country, country);
            })
            countriesList = [...policyData.keys()];
            countriesList.forEach((country) => selectItems.push({ value:country, label:country }));
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
    {#if mapData.features && policyData.size > 0 && selectItems.length > 0}
        <div id="maparea">
            <div id="selector" class="select-theme">
                <label for="country-select">Select a country:</label>
                <Select class="country-select" 
                name="country-select" 
                items={selectItems}
                bind:value={selectedCountry}
                searchable={false}
                clearable={false}
                containerStyles="font-family: Inter !important;"
                 showChevron />
            </div>
            <div id="map">
                <WorldMap mapData={mapData} 
                bind:selectedCountry={selectedCountry} 
                countriesList={countriesList}/>
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
    
    #maparea { width: 100%; }

    #map { width: 100%; }

    #info { width: 95%;
    margin-left: auto;
    margin-right: auto; }

    .layout {
        display: block; 
        gap: 1rem;      
        margin-top: 1rem;
        text-align: center;
    }

    #selector {
        display: flex;
        gap: 0.5rem;
        margin: 0 auto; 
        width: fit-content;
    }

    label {
        font-family: Inter;
        color: #0D534D;
        font-weight: bold;
        padding-top: 10px;
        font-size: 18px;
    }

    .select-theme {
        --item-is-active-bg: #0e534d;
        --item-hover-bg: #fedeb4;
        --list-max-height: 600px;
        --list-border: 1px solid white;
    }

    :global(input) {
        font-family: Inter !important;
    }

    :global(.country-select) {
        font-family: Inter !important;
        font-size: 16px !important;
        border-radius: 10px !important;
        border: 1px solid rgb(186, 186, 186) !important;
        background-color: #ffffff !important;
        margin-bottom: 1rem !important;
        width: 265px !important;
        text-align: left;
    }
    
    @media (min-width: 1000px) {
        #maparea { width: 55% }
        #map { width: 100% }

        #info { width: 45% }

        .layout {
            gap: 1rem;
            display: flex;
        }

        #selector {
            padding-top: 0;
        }
    }

    @media (min-width: 1200px) {
        #maparea { width: 60% }
        #map { width: 100% }

        #info { width: 40% }

        .layout {
            display: flex;
            gap: 1rem;
        }

        #selector {
            padding-top: 0;
        }
    }

    @media (max-width: 500px) {
        #selector {
            display: block;
        }

        label {
            display: block;
        }

        .layout {
            margin-top: 0;
        }

        :global(p, td, th) {
            font-size: 14px !important;
        }
    }
</style>