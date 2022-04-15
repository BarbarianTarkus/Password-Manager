<!-- https://eugenkiss.github.io/7guis/tasks#crud -->

<script>
    import { onMount } from 'svelte'
    import axios from 'axios'
    
    const endpoint = 'http://localhost:3000/movies'

    let movies = []

    onMount(async() => {
        try{
            const response = await axios.get(endpoint)
            movies = await response.data
            console.log(movies)
        } catch (error){
            console.error(error)
        }
    })

    let prefix = '';
    let nom = ''
    let data = ''
    let i = 0
    
    $: filteredMovies = prefix
        ? movies.filter( movies=> {
            const nom = `${movies.nom}`;
            return nom.toLowerCase().startsWith(prefix.toLowerCase())
        })
        : movies;

    $: selected = filteredMovies[i];
     
    $: reset_inputs(selected);
    
    function create() {
        movies = movies.concat({ nom, data });
        i = movies.length - 1;
        nom = data = '';
    }
    function remove() {
        // Remove selected person from the source array (people), not the filtered array
        const index = movies.indexOf(selected);
        movies = [...movies.slice(0, index), ...movies.slice(index + 1)];

        nom = data = '';
        i = Math.min(i, filteredMovies.length - 2);
    }
    function update() {
        selected.nom = nom;
        selected.data = data;
        movies = movies;
    }
    function reset_inputs(movies) {
        nom = movies ? movies.nom : '';
        data = movies ? movies.data : '';
    }
</script>


<input placeholder="filter prefix" bind:value={prefix}>

<select bind:value={i} size={5}>
    {#each movies as movie, i}
        <option value={i}>{movie.nom}, {movie.data}</option>
    {:else}
        <p>loading...</p>
    {/each}
</select>

<label><input bind:value={nom} placeholder="nom"></label>
<label><input bind:value={data} placeholder="data"></label>


<div class='buttons'>
    <button on:click={create} disabled="{!nom || !data}">create</button>
    <button on:click={update} disabled="{!nom || !data || !selected}">update</button>
    <button on:click={remove} disabled="{!selected}">delete</button>
</div>



<style>
    * {
        font-family: inherit;
        font-size: inherit;
    }

    input {
        display: block;
        margin: 0 0 0.5em 0;
    }

    select {
        float: left;
        margin: 0 1em 1em 0;
        width: 14em;
    }

    .buttons {
        clear: both;
    }
</style>