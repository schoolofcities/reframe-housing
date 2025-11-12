<script>
    export let mapData;
	export let selectedCountry;
    export let countriesList;

    import { select, selectAll } from 'd3-selection';
    import { geoPath, geoNaturalEarth1 } from 'd3-geo';
    
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

<div bind:clientWidth={width} id="mapcontainer">
    {#if mapData && mapData.features && width}
        <svg width={width} height={height}>
            <path
                d={path({type: 'Sphere'})}
                id="water"
            />
            {#each mapData.features as country (country.properties.NAME_EN)}
                <path
                    d={path(country)}
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
            <text x={7} y={height - 30} >Data:</text>
            <text x={7} y={height - 10} >Natural Earth</text>
        </svg>
    {/if}
</div>

<style>
    #mapcontainer {
        width: 100%;
        height: auto;
    }
    .country {
        fill: #dfdfdf;
        stroke: grey;
    }
    
    .clickable {
        fill: #fedeb4;
        cursor: pointer;
    }

    .hovered {
        fill: #ffc054;
    }

    .selected {
        fill: #0e534d;
    }

    #water {
        stroke: grey;
        fill: #f5f5f5    
    }

    text {
        font-size: 12px;
    }
</style>
