<script>
    import { onMount } from "svelte";
    import axios from "axios";
    import AutoComplete from "simple-svelte-autocomplete";

    const endpoint = "http://localhost:3000/carpetes";

    let carpetes = [];

    async function getAllElements() {
        try {
            const response = await axios.get(endpoint);
            carpetes = await response.data;
        } catch (error) {
            console.error(error);
        }
    }

    onMount(getAllElements);

    let prefix = "";

    let nom_carpeta = "";
    let pare = "";
    let i = null;

    $: filteredcarpetes = prefix
        ? carpetes.filter((carpetes) => {
              const carpeta = `${carpetes.nom_carpeta}, ${carpetes.pare}`;
              return carpeta.toLowerCase().startsWith(prefix.toLowerCase());
          })
        : carpetes;

    $: selected = filteredcarpetes[i];

    $: reset_inputs(selected);

    async function create() {
        const carpeta = {
            nom_carpeta: nom_carpeta,
            pare: pare
        };

        try {
            const response = await axios.post(endpoint, carpeta);
            console.log(response);
            carpetes = carpetes.concat({ nom_carpeta, pare });
            i = carpetes.length - 1;
            nom_carpeta = pare = "";
        } catch (error) {
            console.error(error);
            
        }

            

    }

    async function remove() {
        const index = carpetes.indexOf(selected);
        try {
            const response = await axios.delete(`http://localhost:3000/carpetes?nom_carpeta=eq.${selected.nom_carpeta}`);
            console.log(response);
        } catch (error) {
            if (error != "TypeError: selected is undefined") {
                console.error(error);
            }
        }
        carpetes = [...carpetes.slice(0, index), ...carpetes.slice(index + 1)];
        nom_carpeta = pare = '';
        i = Math.min(i, filteredcarpetes.length - 2);
    }

    async function update() {
        const carpeta = {
            nom_carpeta: nom_carpeta,
            pare: pare,
        };

        try {
            const response = await axios.patch(
                `http://localhost:3000/carpetes?nom_carpeta=eq.${selected.nom_carpeta}`,
                carpeta
            );
            console.log(response);
        } catch (error) {
            if (error != "TypeError: selected is undefined") {
                console.error(error);
            }
        }
        selected.nom_carpeta = nom_carpeta;
        selected.pare = pare;
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



<input placeholder="Cerca..." bind:value={prefix} />


<select bind:value={i} size={5}>
    {#each filteredcarpetes as carpeta, i}
        <option value={i}>{carpeta.nom_carpeta} [{carpeta.pare}] </option>
    {:else}
        <p>loading...</p>
    {/each}
</select>
<label>
    <button class="reset" on:click={reset_inputs} disabled={!nom_carpeta || !selected}>reset</button>
</label>
<table>
    <tr>
        <th>Nom</th>
        <th>Pare</th>
    </tr>
    <tr>
        <td data-th="nom_carpeta">
            <input type="text" bind:value={nom_carpeta} placeholder=nom>
        </td>
        <td data-th="pare">
            <input bind:value={pare} placeholder="pare" />
        </td>
    </tr>
</table>

<div class="buttons">
    <button on:click={create} disabled={!nom_carpeta}>Crea</button>
    <button
        on:click={update}
        disabled={!nom_carpeta || !selected}>Actualitza</button>
    <button on:click={remove} disabled={!nom_carpeta ||!selected}>Esborra</button>
</div>