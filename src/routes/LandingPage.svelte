<script>
    import { Router, Route, Link } from "svelte-navigator";
    import LandingComponent from "../components/LandingComponent.svelte";
    import AboutComponent from "../components/AboutComponent.svelte";
    import ContactComponent from "../components/ContactComponent.svelte";
</script>

<main>
    <div class="wrapper">
        <Router>
            <div class="container ui grid" style="height: 100vh;">
                <div class="sixteen wide column" />
                <div class="re fourteen wide column">
                    <div class="compNavContainer">
                        <div class="compNav">
                            <Link to="/">
                                <div class="link">Home</div>
                            </Link>
                        </div>
                        <div class="compNav">
                            <Link to="/about">
                                <div class="link">About</div>
                            </Link>
                        </div>

                    </div>
                </div>

                <div class="gree two wide column">
                    <div class="compNav">
                        <Link to="/login">
                            <div class="link">
                                <i class="user icon" /> Login
                            </div>
                        </Link>
                    </div>
                </div>

                <div class="re sixteen wide column" />

                <div class="sixteen wide column">
                    <Route path="/">
                        <LandingComponent />
                    </Route>

                    <Route path="/about">
                        <AboutComponent />
                    </Route>

                    <Route path="/contact">
                        <ContactComponent />
                    </Route>
                </div>

                <!-- footer  -->
                <div class="re sixteen wide column" />
                <div class="re sixteen wide column" />
                <div class="re sixteen wide column" />
                <div class="re sixteen wide column" />
                <div class="re sixteen wide column" />
                <div class="re sixteen wide column" />
                <div class="re sixteen wide column" />

                <div class="gree sixteen wide column" />
                <!-- footer  -->
            </div>
        </Router>
    </div>

    <div class="copyright" style="text-align: center;padding:1em;">
        &copy; 2021 All Rights Reserved
    </div>
</main>

<style>
    main {
        height: 100vh;
        margin-top: -2em;
        background-image: url("../assets/images/images.jpeg");
        background-size: cover;
    }

    .wrapper {
        height: 100%;
        background: rgba(0, 0, 0, 0.76);
    }

    .container {
        max-width: 80em;
        margin: 2em auto !important;
    }

    .compNavContainer {
        display: flex;
    }

    .link {
        color: aliceblue;
        margin-inline: 1em;
        font-weight: 600;
        font-size: 15px;
    }
</style>
