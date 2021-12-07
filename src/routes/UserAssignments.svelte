<script>
    import { onMount } from "svelte";
    import baseUrl from "../config/config";

    let assignments;

    const getMyAssignments = () => {
        let liu = {
            liu: window.localStorage.shmsLiu,
        };

        fetch(baseUrl + "get-user-assignments.php", {
            method: "POST",
            body: JSON.stringify(liu),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                assignments = data;
            })
            .catch((e) => {
                console.log(e);
            });
    };
    onMount(() => {
        getMyAssignments();
    });
</script>

<main>
    <div class="wrapper">
        <div class="ui grid" style="background: rgb(34, 34, 34);;">
            <div class="titleBar re sixteen wide column">
                <span style="cursor: pointer;color:dodgerblue">
                    <i class="arrow left icon" /> Back
                </span>

                &nbsp;&nbsp;&nbsp;&nbsp; My Assignments
            </div>

            <div class="sixteen wide column">
                <div class="">
                    <table class="ui inverted striped basic table">
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Hall</th>
                                <th>Seat</th>
                                <th>Course Code</th>
                                <th>Date/Time</th>
                                <th>Invigilator</th>
                            </tr>
                        </thead>
                        {#if assignments != undefined}
                            <tbody>
                                {#each assignments as assignment}
                                    <tr>
                                        <td>
                                            {assignment.no}
                                        </td>
                                        <td>
                                            {assignment.hostel}
                                        </td>
                                        <td>
                                            {assignment.room}
                                        </td>
                                        <td>
                                            {assignment.date}
                                        </td>
                                    </tr>
                                {/each}
                            </tbody>
                        {/if}
                    </table>
                </div>
            </div>
        </div>
    </div>
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
