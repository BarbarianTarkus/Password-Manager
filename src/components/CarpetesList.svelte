<script>
    import { onMount } from "svelte";
    import axios from "axios";
    import AutoComplete from "simple-svelte-autocomplete";

    const endpoint = "http://localhost:3000/carpetes";

    let carpetes = [];

    onMount(async () => {
        try {
            const response = await axios.get(endpoint);
            carpetes = await response.data;
            //console.log(carpetes);
        } catch (error) {
            //console.error(error);
        }
    });

    let prefix = "";

    let nom_carpeta = "";
    let pare = "";
    let i = 0;

    $: filteredcarpetes = prefix
        ? carpetes.filter((carpetes) => {
              const carpeta = `${carpetes.nom_carpeta}, ${carpetes.pare}`;
              return carpeta.toLowerCase().startsWith(prefix.toLowerCase());
          })
        : carpetes;

    $: selected = filteredcarpetes[i];

    $: reset_inputs(selected);

    function create() {
        carpetes = carpetes.concat({ nom, data });
        i = carpetes.length - 1;
        usuari_carpeta = nom_carpeta = "";
    }

    function remove() {
        // Remove selected person from the source array (people), not the filtered array
        const index = carpetes.indexOf(selected);
 
        carpetes = [...carpetes.slice(0, index), ...carpetes.slice(index + 1)];
        nom_carpeta = pare = "";
        i = Math.min(i, filteredcarpetes.length - 2);
    }

    function update() {
        selected.usuari_carpeta = usuari_carpeta;
        selected.nom_carpeta = nom_carpeta;
        carpetes = carpetes;
    }

    function reset_inputs(carpetes) {
        nom_carpeta = carpetes ? carpetes.nom_carpeta : "";
        pare = carpetes ? carpetes.pare : "";
    }
    //console.log(usuari_carpeta);
    let selectedcarpetaObject = carpetes[0];
    let selectedcarpetaValue = "";
    let highlightedcarpetaObject = "";
</script>



<input placeholder="filter prefix" bind:value={prefix} />
<label>
    <select bind:value={i} size={5}>
        {#each filteredcarpetes as carpeta, i}
            <option value={i}
                >{carpeta.nom_carpeta} [{carpeta.pare}]</option
            >
        {:else}
            <p>loading...</p>
        {/each}
    </select>
</label>

<table>
    <tr>
        <th>nom_carpeta</th>
        <th>pare</th>
    </tr>
    <tr>
        <td data-th="nom_carpeta">
            {nom_carpeta}
        </td>
        <td data-th="pare">
            <input bind:value={pare} placeholder="pare" />
        </td>
    </tr>
</table>

<div class="buttons">
    <button on:click={create} disabled={!nom_carpeta || !nom_carpeta}>create</button>
    <button
        on:click={update}
        disabled={!nom_carpeta || !pare || !selected}>update</button>
    <button on:click={remove} disabled={!selected}>delete</button>
</div>


<style>

    input {
		display: block;
		margin: 0 0 0.5em 0;
	}
    select {
        float: left;
        margin: 0 1em 1em 0;
        width: 20em;
    }

    label {
        position: relative;
        display: flex;
        flex-wrap: wrap;

        color: hsl(240, 25%, 20%);
        background: hsla(240, 25%, 95%, 1);
        padding: 0.75rem 1rem;
        border-radius: 10px;
        width: 20em;
        margin: 1em 0em;
    }

    table {
        background: #34495e;
        color: #fff;
        border-radius: 0.4em;
        overflow: hidden;
    }
    table th {
        color: #dd5;
    }

    tr {
        border-color: lighten(#34495e, 10%);
        text-align: left;
    }
    th,
    td {
        margin: 0.5em 1em;
        padding: 0.5em 2em;
    }

    .buttons {
        clear: both;
    }
</style>
