<script>
    import { onMount } from 'svelte'
    let movies = []

    onMount(async() => {
        const response = await fetch('http://localhost:3000/movies')
        movies = await response.json()
        console.log(movies)
    })
</script>