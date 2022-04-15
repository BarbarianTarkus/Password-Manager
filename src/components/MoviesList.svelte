<script>
    import { onMount } from 'svelte'
    let movies = []

    onMount(async() => {
        const response = await fetch('http://localhost:3000/movies')
        movies = await response.json()
        console.log(movies)
    })
    let i = 0
</script>

<select bind:value={i} size={5}>
{#each movies as movie, i}
    <option value={i}>{movie.nom}, {movie.data}</option>
{:else}
    <p>loading...</p>
{/each}

</select>

