<script>
    import { onMount } from "svelte";
    import axios from "axios";

    const endpoint = "http://localhost:3000/comptes";

    let comptes = [];

    async function getAllElements() {
        try {
            const response = await axios.get(endpoint);
            comptes = await response.data;
        } catch (error) {
            console.error(error);
        }
    }

    onMount(getAllElements);

    let prefix = "";
    let usuari_compte = "";
    let clau_mestra = "";
    let nom_compte = "";
    let cognoms = "";
    let nom_carpeta = "";
    let i = null;

    $: filteredComptes = prefix
        ? comptes.filter((comptes) => {
              const compte = `${comptes.usuari_compte}, ${comptes.nom_compte}`;
              return compte.toLowerCase().startsWith(prefix.toLowerCase());
          })
        : comptes;

    $: selected = filteredComptes[i];

    $: reset_inputs(selected);

    async function create() {
        const compte = {
            usuari_compte: usuari_compte,
            nom_compte: nom_compte,
            clau_mestra: clau_mestra,
            cognoms: cognoms,
        };
        try {
            const response = await axios.post(endpoint, compte);
            console.log(response);
            comptes = comptes.concat({ usuari_compte, nom_compte, clau_mestra,cognoms });
            i = comptes.length - 1;
            usuari_compte = nom_compte = clau_mestra = cognoms = "";
        } catch (error) {
            console.error(error);
            
        }

            
    }
    async function remove() {
        const index = comptes.indexOf(selected);
        try {
            const response = await axios.delete(endpoint + `?usuari_compte=eq.${selected.usuari_compte}`);
            console.log(response);
        } catch (error) {
            if (error != "TypeError: selected is undefined") {
                console.error(error);
            }
        }
        comptes = [...comptes.slice(0, index), ...comptes.slice(index + 1)];
        usuari_compte = nom_compte = '';
        i = Math.min(i, filteredComptes.length - 2);
    }
    async function update() {
        const compte = {
            usuari_compte: usuari_compte,
            nom_compte: nom_compte,
            clau_mestra: clau_mestra,
            cognoms: cognoms,
        };

        try {
            const response = await axios.patch(endpoint +
                `?usuari_compte=eq.${selected.usuari_compte}`,
                compte
            );
            console.log(response);
        } catch (error) {
            if (error != "TypeError: selected is undefined") {
                console.error(error);
            }
        }
        selected.usuari_compte = usuari_compte;
        selected.nom_compte = nom_compte;
        comptes = comptes;
    }

    function reset_inputs(comptes) {
        usuari_compte = comptes ? comptes.usuari_compte : "";
        clau_mestra = comptes ? comptes.clau_mestra : "";
        nom_compte = comptes ? comptes.nom_compte : "";
        cognoms = comptes ? comptes.cognoms : "";
        nom_carpeta = comptes ? comptes.nom_carpeta : "";
    }
    //console.log(usuari_compte);
    let selectedCompteObject = comptes[0];
    let selectedCompteValue = "";
    let highlightedCompteObject = "";



    function revealPassword(){
        var x = document.getElementById("reveal");
        var y = document.getElementById("buttonReveal");
        if (x.type === "password") {
            x.type = "text";
            y.style = "background-color: rgb(0, 0, 0);";
        } else {
            x.type = "password";
            y.style = "background-color: rgb(255, 255, 255);";
        }

    }
</script>



<input class="filter" placeholder="Cerca..." bind:value={prefix} />



<select bind:value={i} size={5}>
    {#each filteredComptes as compte, i}
        <option value={i}>{compte.usuari_compte} [{compte.nom_compte}]</option>
    {:else}
        <p>loading...</p>
    {/each}
</select>

<label>
    <button class="reset" on:click={reset_inputs} disabled={!selected}>Neteja</button>
    <table>
        <tr>
            <th>Usuari</th>
            <th>Clau Mestra</th>
            <th>Nom</th>
            <th>Cognoms</th>
            <th>Accés a carpetes</th>
        </tr>
        <tr>
            <td data-th="Usuari">
                <input for="usuari-compte" type="text" bind:value={usuari_compte} placeholder="*Usuari" required/>
            </td>
            <td data-th="Clau Mestra">
                <input for="clau-mestra" type="password" bind:value={clau_mestra} placeholder="*Clau Mestra" id="reveal"/>            
                <button style="display:inline" on:click={revealPassword} id="buttonReveal" required/> 
            </td>
            <td for="nom_compte" type="text" data-th="Nom">
                <input bind:value={nom_compte} placeholder="Nom"/>
            </td>
            <td for="cognoms" type="text" ata-th="Cognoms">
                <input bind:value={cognoms} placeholder="Cognoms" />
            </td>
            <td data-th="Accés a carpetes"> 
                {nom_carpeta}
            </td>
        </tr>
    </table>
</label>


<div class="buttons">
    <button on:click={create} disabled={!usuari_compte || !clau_mestra}>Crea</button>
    <button
        on:click={update}
        disabled={!usuari_compte || !clau_mestra || !selected}>Actualitza</button>
    <button on:click={remove} disabled={!selected}>Esborra</button>
    
</div>

