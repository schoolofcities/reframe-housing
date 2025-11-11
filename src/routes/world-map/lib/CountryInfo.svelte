<script>
    export let policyData;
    export let selectedCountry;

    import { marked } from 'marked'
</script>

<div>
    {#if policyData && selectedCountry}
        <h2>{selectedCountry}</h2>
        <table>
            <tbody>
                <tr>
                    <th>Doing or Paying?</th>
                    <td>{policyData.get(selectedCountry).doing_or_paying}</td>
                </tr>
                <tr>
                    <th>Tools</th>
                    <td>{policyData.get(selectedCountry).tools}</td>
                </tr>
                <tr>
                    <th>Priorities</th>
                    <td>{policyData.get(selectedCountry).priorities}</td>
                </tr>
            </tbody>
        </table>
        <p class="description">{@html marked(policyData.get(selectedCountry).description)}</p>
    {/if}
</div>