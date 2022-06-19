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
    let director = ''
    let puntuacio = ''
    let i = 0
    
    $: filteredMovies = prefix
        ? movies.filter( movies=> {
            const movie = `${movies.nom}, ${movies.data}`
            return movie.toLowerCase().startsWith(prefix.toLowerCase())
        })
        : movies
    
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
        director = movies ? movies.director : '';
        puntuacio = movies ? movies.puntuacio : '';
    }
    console.log(nom)
    let selectedMovieObject = movies[0]
    let selectedMovieValue = ''
    let highlightedMovieObject = ''
</script>



<label>
    <select bind:value={i} size={5}>
        {#each filteredMovies as movie, i}
            <option value={i}>{movie.nom} [{movie.data}]</option>
        {:else}
            <p>loading...</p>
        {/each}
    </select>
</label>


<table>
    <tr>
        <th>Name</th>
        <th>Release</th>
        <th>Director</th>
        <th>Score</th>
    </tr>
    <tr>
        <td data-th="nom">
            {nom}
        </td>
        <td data-th="data">{data}</td>
        <td data-th="director">{director}</td>
        <td data-th="puntuacio">{puntuacio}</td>
    </tr>
</table>

<div class='buttons'>
    <button on:click={create} disabled="{!nom || !data}">create</button>
    <button on:click={update} disabled="{!nom || !data || !selected}">update</button>
    <button on:click={remove} disabled="{!selected}">delete</button>
</div>


<style>
    select {
        float: left;
        margin: 0 1em 1em 0;
        width: 20em;
    }

    label {
        position:relative;
        display: flex;
        flex-wrap: wrap;
        align-items: center;
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
        margin: 1em 0;
        padding: 2em 0em;
    }
    table th{
        color: #dd5;
    }

    tr {
        border-color: lighten(#34495e, 10%);
        text-align: left;
    }
    th,td {
        margin: 0.5em 1em;
        padding: 0.5em 2em;

    }

    .buttons {
        clear: both;
    }
    
</style>
