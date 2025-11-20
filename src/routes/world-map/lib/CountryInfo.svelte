<script>
    export let policyData;
    export let selectedCountry;

    import { marked } from 'marked'
</script>

<div id="country-info">
    {#if policyData && selectedCountry}
        <h2>{selectedCountry.value}</h2>
        <table style="width:100%">
            <tbody>
                <tr>
                    <th style="width:30%">Doing or Paying?</th>
                    <td>{@html marked(policyData.get(selectedCountry.value).doing_or_paying).replace(/<\/?p>/g,"")}</td>
                </tr>
                <tr>
                    <th>Tools</th>
                    <td>{@html marked(policyData.get(selectedCountry.value).tools).replace(/<\/?p>/g,"")}</td>
                </tr>
                <tr>
                    <th>Priorities</th>
                    <td>{@html marked(policyData.get(selectedCountry.value).priorities).replace(/<\/?p>/g,"")}</td>
                </tr>
            </tbody>
        </table>
        <p id="description">{@html marked(policyData.get(selectedCountry.value).description).replace(/<\/?p>/g,"")}</p>
    {/if}
</div>

<style>
    #country-info {
        align-items: center;
        text-align: center;
        margin: 1rem;
        background-color: #0D534D;
        border-radius: 10px;
    }

    table {
        border-collapse: separate; 
        border-spacing: 0.5rem;
        margin-top: 0.5rem;
    }

    th {
        text-align: right;
        font-family: Inter;
        font-weight: bold;
        color: #f5f5f5;
        padding-top: 0.5rem;
    }

    td {
        text-align: left;
        padding-left: 1rem;
        padding-right: 3rem;
        padding-top: 0.5rem;
        font-family: Inter;
        color: #f5f5f5;
        line-height: 26px;
    }

    h2 {
        font-family: TradeGothicBold;
        text-align: center;
        background-color: #fedeb4;
        display:inline-block;
        padding:5px 15px;
        border-radius: 10px;
        color: #0D534D;
        margin-top: 1rem;
        text-wrap: stable;
    }

    #description {
        margin-top: 1rem;
        padding: 0 1rem 1rem 1rem;
    }

    p {
        font-size: 1rem;
        color: #f5f5f5;
        line-height: 26px;
    }

    @media (min-width: 1100px) {
        td {
            text-align: left;
            padding-left: 1rem;
            padding-right: 1rem;
            font-family: Inter;
        }
    }

    @media (max-width: 500px) {
        h2 {
            font-size: 25px;
        }
    }


</style>