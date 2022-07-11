<script>
    import { Link } from "svelte-navigator";
</script>

<nav>
    <h2 class="headerTitle" on:click="{() => window.location.pathname = "/"}">
        GiphSvelt
    </h2>
    <ul>
        <li>
            <Link class="link" to="/">
                Home
            </Link>
        </li>
        <li>
            <Link class="link" to="/search">
                Search
            </Link>
        </li>
        <li>
            <Link class="link" to="/login">
                Login
            </Link>
        </li>
        <li>
            <Link class="link" to="/signup">
                Signup
            </Link>
        </li>
    </ul>
</nav>

<style>
    .headerTitle {
        cursor: pointer;
    }
    nav {
        width: 100vw;
        display: flex;
        position: fixed;
        top: 0;
        left: 0;
        z-index: 100;
        background-color: beige;
        align-items: center;
        justify-content: space-between;
        padding-right: 10vw;
    }
    h2 {
        margin-left: 3vw;
    }
    ul {
        display: flex;
        width: 40vw;
        justify-content: space-evenly;
    }
    li {
        list-style: none;
    }
    li :global(.link) {
        color: black;
        text-decoration: none;
    }
</style>