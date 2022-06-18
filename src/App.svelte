<!-- https://eugenkiss.github.io/7guis/tasks#crud -->
<script>
import {Router, Route, Link} from "svelte-routing";
import ComptesList from "./components/ComptesList.svelte";
import CarpetesList from "./components/CarpetesList.svelte";
import TableStyles from "./components/TableStyles.svelte";
</script>

<style>
    :global(*) {
            box-sizing: border-box;
            padding: 0;
            margin: 0 1em;
    }
    :global(body) {
        color: hsl(0, 0%, 51%);
        background: rgba(40, 39, 49, 0.994)    }
    header {
        margin: 0em -1em;
    }
    h3{
        margin: 0 0;
        margin-top: 3em;
       
    }
</style>
<TableStyles/>

<Router>
    <header>
        <h1>LockMaster</h1>
        <nav>
            <Link to="/">Home</Link>
            <Link to="comptes">Comptes</Link>
            <Link to="carpetes">Carpetes</Link>
        </nav>
    </header>
    
    <main>
        <Route path="/">
            <h3>Home</h3>
            <p>This is the home page.</p>
        </Route>
    
        <Route path="comptes">
            <ComptesList />
        </Route>
    
        <Route path="carpetes">
            <CarpetesList />
        </Route>
    </main>
</Router>

