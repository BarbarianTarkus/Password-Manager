<script>
    import { onMount } from "svelte";
    import axios from "axios";
    import AutoComplete from "simple-svelte-autocomplete";

    const endpoint = "http://localhost:3000/comptes";

    let comptes = [];

    onMount(async () => {
        try {
            const response = await axios.get(endpoint);
            comptes = await response.data;
            //console.log(comptes);
        } catch (error) {
            //console.error(error);
        }
    });

    let prefix = "";
    let usuari_compte = "";
    let clau_mestra = "";
    let nom_compte = "";
    let cognoms = "";
    let i = 0;

    $: filteredComptes = prefix
        ? comptes.filter((comptes) => {
              const compte = `${comptes.usuari_compte}, ${comptes.nom_compte}`;
              return compte.toLowerCase().startsWith(prefix.toLowerCase());
          })
        : comptes;

    $: selected = filteredComptes[i];

    $: reset_inputs(selected);

    function create() {
        comptes = comptes.concat({ nom, data });
        i = comptes.length - 1;
        usuari_compte = nom_compte = "";
    }

    async function remove() {
        // Remove selected person from the source array (people), not the filtered array
        const index = comptes.indexOf(selected);
        try {
            const response= await axios.delete(`http://localhost:3000/comptes/${index}`);
            console.log(response);
        } catch (error) {
            console.error(error);
        }
        
        /*
        comptes = [...comptes.slice(0, index), ...comptes.slice(index + 1)];
        usuari_compte = nom_compte = "";
        i = Math.min(i, filteredComptes.length - 2);
        */
    }

    function update() {
        selected.usuari_compte = usuari_compte;
        selected.nom_compte = nom_compte;
        comptes = comptes;
    }

    function reset_inputs(comptes) {
        usuari_compte = comptes ? comptes.usuari_compte : "";
        clau_mestra = comptes ? comptes.clau_mestra : "";
        nom_compte = comptes ? comptes.nom_compte : "";
        cognoms = comptes ? comptes.cognoms : "";
    }
    //console.log(usuari_compte);
    let selectedCompteObject = comptes[0];
    let selectedCompteValue = "";
    let highlightedCompteObject = "";

    onMount(remove);

</script>



<input placeholder="filter prefix" bind:value={prefix} />
<label>
    <select bind:value={i} size={5}>
        {#each filteredComptes as compte, i}
            <option value={i}
                >{compte.usuari_compte} [{compte.nom_compte}]</option
            >
        {:else}
            <p>loading...</p>
        {/each}
    </select>
</label>

<table>
    <tr>
        <th>usuari_compte</th>
        <th>clau_mestra</th>
        <th>nom_compte</th>
        <th>cognoms</th>
    </tr>
    <tr>
        <td data-th="usuari_compte">
            {usuari_compte}
        </td>
        <td data-th="clau_mestra">
            <input bind:value={clau_mestra} placeholder="clau_mestra" />
        </td>
        <td data-th="nom_compte">{nom_compte}</td>
        <td data-th="cognoms">{cognoms}</td>
    </tr>
</table>

<div class="buttons">
    <button on:click={create} disabled={!usuari_compte || !nom_compte}>create</button>
    <button
        on:click={update}
        disabled={!usuari_compte || !nom_compte || !selected}>update</button>
    <button on:click={remove} disabled={!selected}>delete</button>
</div>


<style>

    input {
		display: block;
		margin:  1em 1em;
	}

    label {

        color: back;
        background: rgba(231, 231, 231, 0.994);
        padding: 0rem 0rem;
        border-radius: 10px;
        width: 20em;
        margin: 0 1em;
    }

    select {
        float: left;
        margin: 0 1em 1em 0;
        width: 20em;

    }
    table {
        background: rgba(118, 116, 130, 0.994);
        color: rgb(255, 255, 255);
        border-radius: 0 1em 1em 0;
        margin: 0em 1em;
    }    
    tr {
        border-color: lighten(#ffffff, 10%);
        text-align: left;
    }
    th,
    td {
        padding: 0.5em 0.5em;
    }


    table th {
        color: rgb(255, 255, 255);
    }


    .buttons {
        clear: both;
        margin: 1em 1em;
    }
</style>
