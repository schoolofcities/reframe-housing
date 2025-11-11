<script>
    export let mapData;
	export let selectedCountry;
    export let countriesList;

    import { select, selectAll } from 'd3-selection';
    import { geoPath, geoNaturalEarth1 } from 'd3-geo';
    import { onMount } from 'svelte';
    
    let width;
    $: height = width * 0.5;

    let projection, path;
    
    $: if (mapData && mapData.features) {
        projection = geoNaturalEarth1()
			.fitExtent([[10, 10], [width - 10, height - 10]], mapData);
		path = geoPath(projection);
    }
    
    let hoveredCountry = null;

    function handleClick(name) {
        if (countriesList.includes(name)) {
            selectedCountry = name;
        }
    }

    function handleMouseEnter(name) {
        if (countriesList.includes(name) && name !== selectedCountry) {
            hoveredCountry = name;
        }
    }

    function handleMouseLeave(name) {
        if (hoveredCountry === name) {
            hoveredCountry = null;
        }
    }
</script>

<div bind:clientWidth={width}>
    {#if mapData && mapData.features && width}
        <svg width={width} height={height}>
            {#each mapData.features as country (country.properties.NAME_EN)}
                <path
                    d={path(country)}
                    stroke="grey"
                    stroke-width="1"
                    id={country.properties.NAME_EN.replaceAll(' ', '-')}
                    class:country={true}
                    class:clickable={countriesList.includes(country.properties.NAME_EN)}
                    class:selected={selectedCountry === country.properties.NAME_EN}
                    class:hovered={hoveredCountry === country.properties.NAME_EN}
                    on:click={() => handleClick(country.properties.NAME_EN)}
                    on:mouseenter={() => handleMouseEnter(country.properties.NAME_EN)}
                    on:mouseleave={() => handleMouseLeave(country.properties.NAME_EN)}
                />
            {/each}
        </svg>
    {/if}
</div>

<style>
    .country {
        fill: white
    }
    
    .clickable {
        fill: lightgrey;
        cursor: pointer;
    }

    .hovered {
        fill: lightgreen
    }

    .selected {
        fill: green
    }
</style>