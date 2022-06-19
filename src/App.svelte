<!-- https://eugenkiss.github.io/7guis/tasks#crud -->
<script>
import {Router, Route, Link} from "svelte-routing";
import ComptesList from "./components/ComptesList.svelte";
import CarpetesList from "./components/CarpetesList.svelte";
import CercasList from "./components/CercasList.svelte";
import CredencialsList from "./components/CredencialsList.svelte";
import DireccionsList from "./components/DireccionsList.svelte";
import RolsList from "./components/RolsList.svelte";
import EnrolamentsList from "./components/EnrolamentsList.svelte";
import TableStyles from "./components/TableStyles.svelte";
import TagsList from "./components/TagsList.svelte";

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
            <Link to="credencials">Credencials</Link>
            <Link to="carpetes">Carpetes</Link>
            <Link to="direccions">Direccions</Link>
            <Link to="cercas">Cercas</Link>
            <Link to="tags">Tags</Link>   
            <Link to="rols">Rols</Link>
            <Link to="enrolaments">Enrolaments</Link>   
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
        <Route path="credencials">
            <CredencialsList />
        </Route>
        <Route path="carpetes">
            <CarpetesList />
        </Route>
        
        <Route path="direccions">
            <DireccionsList />
        </Route>
        <Route path="tags">
            <TagsList />
        </Route>
        <Route path="direccions">
            <DireccionsList />
        </Route>
        <Route path="cercas">
            <CercasList />
        </Route>
        <Route path="rols">
            <RolsList />
        </Route>
        <Route path="enrolaments">
            <EnrolamentsList />
        </Route>
    </main>
</Router>

