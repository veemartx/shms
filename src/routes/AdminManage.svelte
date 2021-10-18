<script>
    import { onMount } from "svelte";

    import { Router, Link } from "svelte-navigator";
    import baseUrl from "../config/config";

    let hostels;

    onMount(() => {
        fetch(baseUrl + "get-hostels.php")
            .then((res) => res.json())
            .then((data) => {
                hostels = data;
                console.log(data);
            })
            .catch((e) => {
                console.log(e);
            });
    });
</script>

<main>
    <Router>
        {#if hostels != undefined}
            <div class="wrapper">
                <div
                    class="ui grid"
                    style="background-color: rgb(34, 34, 34);;"
                >
                    <div class="titleBar re sixteen wide column">
                        <span
                            on:click={() => {
                                window.history.back();
                            }}
                            style="cursor: pointer;color:dodgerblue"
                        >
                            <i class="arrow left icon" /> Back
                        </span>

                        &nbsp;&nbsp;&nbsp;&nbsp; Manage

                        <span style="float:right;">
                            <Link to="/admin/dash/manage/hostels/add">
                                <span style="color:cyan"
                                    ><i class="plus icon" /> Add Hall</span
                                >
                            </Link>
                        </span>
                    </div>

                    <div class="yello sixteen  wide column">
                        <b>
                            <h4>Hostels</h4>
                        </b>
                    </div>

                    <div class="re four wide column">
                        <div class="ui vertical inverted menu">
                            <div class="item">
                                <Link to="assignments"
                                    >Assignments</Link
                                >
                            </div>

                            <div class="item">
                                <!-- <Link to="transactions"
                                    >Transactions</Link
                                > -->
                            </div>
                        </div>
                    </div>

                    <div class="twelve wide column">
                        <table class="ui basic inverted striped table">
                            <thead>
                                <tr>
                                    <th> No </th>
                                    <th> Name </th>
                                    <th> Location </th>
                                    <th> Rooms </th>
                                    <th> Vacant </th>
                                    <th> View </th>
                                </tr>
                            </thead>
                            <tbody>
                                {#each hostels as hostel}
                                    <tr>
                                        <td>
                                            {hostel.no}
                                        </td>
                                        <td>
                                            {hostel.name}
                                        </td>
                                        <td>
                                            {hostel.location}
                                        </td>
                                        <td>
                                            {hostel.rooms}
                                        </td>
                                        <td>
                                            {hostel.vacant}
                                        </td>

                                        <th>
                                            <Link
                                                to={"/admin/dash/manage/hostels/" +
                                                    hostel.id}>View</Link
                                            >
                                        </th>
                                    </tr>
                                {/each}
                            </tbody>
                        </table>
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
    }

    .titleBar {
        border-bottom: 1px solid rgba(128, 128, 128, 0.459);
        font-size: 16px;
        font-weight: 600;
    }
</style>
