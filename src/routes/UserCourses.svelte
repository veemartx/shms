<script>
    import { onMount } from "svelte";
    import { Router, Link } from "svelte-navigator";
    import baseUrl from "../config/config";

    let courses;

    const getcourses = () => {
        fetch(baseUrl + "get-courses.php")
            .then((res) => res.json())
            .then((data) => {
                // console.log(data);
                courses = data;
            })
            .catch((e) => {
                console.log(e);
            });
    };

    onMount(() => {
        getcourses();
    });
</script>

<main>
    <Router>
        {#if courses != undefined}
            <div class="wrapper">
                <div class="ui grid">
                    <div class="titleBar re sixteen wide column">
                        <span
                            on:click={() => {
                                window.history.back();
                            }}
                            style="cursor: pointer;color:dodgerblue"
                        >
                            <i class="arrow left icon" /> Back
                        </span>

                        &nbsp;&nbsp;&nbsp;&nbsp;My Courses
                    </div>

                    <div class="sixteen wide column">

                    </div>
                </div>
            </div>
        {/if}
    </Router>
</main>

<style>
    .wrapper {
        margin-left: 1em;
        margin-top: 1em;
        background: rgb(34, 34, 34);
    }

    .titleBar {
        border-bottom: 1px solid rgba(128, 128, 128, 0.459);
        font-size: 16px;
        font-weight: 600;
    }
</style>
