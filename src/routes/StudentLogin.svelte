<script>
    import { Router, Link } from "svelte-navigator";
</script>

<main>
    <div class="wrapper">
        <div class="ui grid">
            <div class="re sixteen wide column">
                <Router>
                    <div class="mainNav">
                        <Link to="/">
                            <i
                                style="color: aliceblue;"
                                class="home icon"
                            /><span class="link"> Home </span>
                        </Link>

                        <Link to="/admin">
                            <i class="user icon" style="color: aliceblue;" />
                            <span class="link"> Admin </span>
                        </Link>
                    </div>
                </Router>
            </div>
        </div>
    </div>
</main>

<style>
    .wrapper {
        max-width: 85em;
        margin: 2em auto;
    }

    .mainNav {
        text-align: right;
    }

    .link {
        color: aliceblue;
        margin-inline: 1em;
        font-weight: 400;
        font-size: 15px;
    }
</style>
