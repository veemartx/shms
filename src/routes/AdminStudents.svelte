<script>
    import { onMount } from "svelte";
    import { Router, Link } from "svelte-navigator";
    import baseUrl from "../config/config";

    let users;

    const getusers = () => {
        fetch(baseUrl + "get-users.php")
            .then((res) => res.json())
            .then((data) => {
                // console.log(data);
                users = data;
            })
            .catch((e) => {
                console.log(e);
            });
    };

    onMount(() => {
        getusers();
    });
</script>

<main>
    <Router>
        {#if users != undefined}
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

                        &nbsp;&nbsp;&nbsp;&nbsp; Students
                        <span style="float:right;">
                            <Link to="/admin/dash/students/add">
                                <span style="color:cyan"
                                    ><i class="plus icon" /> Add Student</span
                                >
                            </Link>
                        </span>
                    </div>

                    <div class="sixteen wide column">
                        <div class="">
                            <table class="ui striped inverted table">
                                <thead>
                                    <tr>
                                        <th> No </th>
                                        <th> Name </th>
                                        <th>Email</th>
                                        <th>Phone</th>
                                        <th> Id No </th>
                                        <th> View </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    {#each users as user}
                                        <tr>
                                            <td>
                                                {user.no}
                                            </td>

                                            <td>
                                                {user.name}
                                            </td>
                                            <td>
                                                {user.email}
                                            </td>
                                            <td>
                                                {user.phone}
                                            </td>
                                            <td>
                                                {user.idNo}
                                            </td>

                                            <td>
                                                <Link to={user.id}>
                                                    <i class="blue eye icon" /> View
                                                </Link>
                                            </td>
                                        </tr>
                                    {/each}
                                </tbody>
                            </table>
                        </div>
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
