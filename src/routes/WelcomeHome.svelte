<script>
    import { Router, Link } from "svelte-navigator";
</script>

<main>
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


                    <Link to="/student">
                        <i
                            style="color: aliceblue;"
                            class="graduation cap icon"
                        /><span class="link"> Student </span>
                    </Link>

                    <Link to="/admin">
                        <i class="user icon" style="color: aliceblue;" />
                        <span class="link"> Admin </span>
                    </Link>
                </div>
            </Router>
        </div>

        <div class="sixteen wide column">
            <div class="title">H.M.S</div>

            <div class="subTitle">Hostel Management System</div>

            <div class="introductionText">
                Lorem ipsum dolor sit amet, consectetur adipisicing elit. Culpa
                mollitia iusto debitis nesciunt ab ratione omnis deleniti rerum
                quos praesentium. Quis provident quo velit exercitationem,
                pariatur praesentium perferendis dignissimos tempora laborum
                magni possimus nulla quibusdam expedita ullam? Accusamus, ullam?
                Molestias soluta omnis qui dolorum laborum fugit voluptatem vero
                obcaecati facilis officia nesciunt consectetur harum magni
                accusamus totam excepturi saepe numquam eligendi nam, velit
                consequuntur itaque modi! Facere sit, repellat doloribus
                molestiae neque perferendis
            </div>
        </div>
    </div>
</main>

<style>
    main {
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

    .title {
        font-size: 50px;
        text-align: center;
        padding-top: 2em;
    }

    .subTitle {
        font-style: italic;
        text-align: center;
        padding-top: 1em;
        font-size: 20px;
        text-decoration: underline;
    }

    .introductionText {
        font-style: normal;
        line-height: 24px;
        padding-top: 3em;
        max-width: 64em;
        margin: auto;
    }
</style>
