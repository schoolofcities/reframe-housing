<script>
    export let mapData;
	export let selectedCountry;
    export let countriesList;

    import { select } from 'd3-selection';
    import { geoPath, geoNaturalEarth1 } from 'd3-geo';
    import { onMount } from 'svelte';
    
    let mapArea;

    function drawMap() {
        if (!mapData || !mapArea) return;
        let width = mapArea.clientWidth;
        let height = mapArea.clientHeight;

        let projection = geoNaturalEarth1()
            .fitSize([width, height], mapData);
        let path = geoPath(projection);

        let map = select(mapArea);
        map.selectAll("*").remove();

        map = map.append("svg")
            .attr("width", '100%')
            .attr("height", '100%')
            .attr('viewBox',`0 0 ${width} ${height}`)
            .attr('preserveAspectRatio','xMinYMin');
        //https://stackoverflow.com/questions/17626555/responsive-d3-chart

        let countries = map.selectAll(".country")
            .data(mapData.features)
            .enter()
            .append("path")
            .attr("d", path)
            .attr("class", "country")
            .attr("fill", (d) => {
                if (countriesList.includes(d.properties.NAME_EN)){
                    return("grey");
                } else {
                    return("white");
                }
            })
            .attr("stroke", "#000")
            .on("click", (event, d) => {
                if (countriesList.includes(d.properties.NAME_EN)){
                    selectedCountry = d.properties.NAME_EN;
                }
                console.log(event);
            });
    };

    onMount(drawMap);
    
    $: if (mapData) drawMap();
</script>

<div>
    <div bind:this={mapArea} style="width:100vw; height: 50vw;"></div>
</div>

<!-- 
<script>
    export let mapData;
	export let selectedCountry;
    export let countriesList;

    import { select } from 'd3-selection';
    import { geoPath, geoNaturalEarth1 } from 'd3-geo';
    import { onMount } from 'svelte';
    
    let width;
    $: height = width * 0.5;

    let projection, path;
    
    $: if (mapData && mapData.features) {
        projection = geoNaturalEarth1()
            .fitSize([width, height], mapData);
		path = geoPath(projection);
    }

    $: console.log("Map ready:", { width, features: mapData?.features?.length });

    function countryClick(event, d) {
        if (countriesList.includes(d.properties.NAME_EN)){
            selectedCountry = d.properties.NAME_EN;
        }
        console.log(d.properties.NAME_EN);
    }
</script>

<div bind:clientWidth={width}>
    {#if mapData && mapData.features && width}
        <svg width="100vw" height="50vw" preserveAspectRatio='xMinYMin'>
            {#each mapData.features as country (country.properties.NAME_EN)}
                <path
                    d={path(country)}
                    stroke="black"
                    fill={countriesList.includes(country.properties.NAME_EN) ? "grey" : "white"}
                    stroke-width="1"
                />
            {/each}
        </svg>
    {/if}
    <p>{selectedCountry}</p>
</div> -->