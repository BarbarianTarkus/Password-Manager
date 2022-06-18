<!-- https://eugenkiss.github.io/7guis/tasks#crud -->
<script>
import ComptesList from "./components/ComptesList.svelte";
import CarpetesList from "./components/CarpetesList.svelte";
import TableStyles from "./components/TableStyles.svelte";
export let menu = 1;
</script>






<style>
    :global(*) {
            box-sizing: border-box;
            padding: 0;
            margin: 0;
    }
    :global(body) {
        color: hsl(0, 0%, 51%);
        background: rgba(40, 39, 49, 0.994)    }
    
    ul#menu li{
		display : inline;
        margin : 0 1em;
	}
</style>



<ul id="menu">
	<li><a href="/comptes" on:click|preventDefault={() => (menu = 1)}>ComptesList</a></li> |
	<li><a href="/carpetes" on:click|preventDefault={() => (menu = 2)}>CarpetesList</a></li>
</ul>


<TableStyles />

{#if menu === 1}
<ComptesList />
{:else if menu === 2}
<CarpetesList />
{:else}
<h1>
	Page Not Found
</h1>
{/if}
