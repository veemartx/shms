<script>
    import { onMount } from "svelte";
    import { Router, link } from "svelte-navigator";
    import { v4 } from "uuid";
    import baseUrl from "../config/config";

    let assignBtnLoading = false;

    let assignmentDetails;

    let rooms;

    let users;

    // form elements
    let hostel;

    let room;

    let user;

    let filteredRooms;

    const handleAssignment = () => {
        let assignmentData = {
            hostel: hostel,
            user: user,
            room: room,
            aid: v4(),
        };

        // send the data to the server

        assignBtnLoading = true;

        fetch(baseUrl + "add-assignment.php", {
            method: "POST",
            body: JSON.stringify(assignmentData),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                // if
                if (data.success) {
                    // toast
                    toast.success(data.message);
                } else {
                    toast.error(data.message);
                }

                getAssignmentDetails();

                assignBtnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                assignBtnLoading = false;
            });

        console.log(assignmentData);
    };

    const handleHostelChange = () => {
        //

        if (hostel != "") {
            // get the rooms that belong to the hostel

            filteredRooms = rooms.filter((room) => {
                // get the hostel name
                return room.hostel == hostel;
            });

            console.log(rooms);

            console.log(filteredRooms);
        }
    };

    const getAssignmentDetails = () => {
        fetch(baseUrl + "get-assignment-details.php")
            .then((res) => res.json())
            .then((data) => {
                assignmentDetails = data;

                rooms = data.rooms;

                filteredRooms = rooms;

                console.log(data);
            })
            .catch((e) => {
                console.log(e);
            });
    };
    onMount(() => {
        getAssignmentDetails();
    });
</script>

<main>
    <Router>
        <div class="wrapper">
            {#if assignmentDetails != undefined}
                <div class="ui grid" style="background: rgb(34, 34, 34);;">
                    <div class="titleBar re sixteen wide column">
                        <span
                            on:click={() => {
                                window.history.back();
                            }}
                            style="cursor: pointer;color:dodgerblue"
                        >
                            <i class="arrow left icon" /> Back
                        </span>

                        &nbsp;&nbsp;&nbsp;&nbsp; Assignments
                    </div>

                    <div class="rightCol four wide column">
                        <div class="">
                            <b>
                                <i class="plus icon" /> Assign Seats
                            </b>

                            <hr />
                        </div>

                        <div class="">
                            <form
                                class="ui form"
                                on:submit|preventDefault={handleAssignment}
                            >
                                <div class="field">
                                    <label for="Hostel">Hall</label>
                                    <select
                                        name="hostel"
                                        id="hostel"
                                        required
                                        bind:value={hostel}
                                        on:change={handleHostelChange}
                                    >
                                        <option value="">Select Hall</option>
                                        {#each assignmentDetails.hostels as hostel}
                                            <option value={hostel.name}
                                                >{hostel.name}</option
                                            >
                                        {/each}
                                    </select>
                                </div>

                                <div class="field">
                                    <label for="room">Seat</label>
                                    <select
                                        name="room"
                                        id="room"
                                        required
                                        bind:value={room}
                                    >
                                        <option value=""
                                            >Select Seat <i
                                                class="circle icon"
                                            />
                                        </option>

                                        {#each filteredRooms as room}
                                            <option value={room.code}
                                                >{room.code}</option
                                            >
                                        {/each}
                                    </select>
                                </div>

                                <div class="field">
                                    <label for="user">User</label>
                                    <select
                                        name="user"
                                        id="user"
                                        required
                                        bind:value={user}
                                    >
                                        <option value="">Select Student</option>

                                        {#each assignmentDetails.users as user}
                                            <option value={user.name}
                                                >{user.name}</option
                                            >
                                        {/each}
                                    </select>
                                </div>

                                <div
                                    class="field"
                                    style="text-align: center;padding-top:2em;"
                                >
                                    {#if assignBtnLoading}
                                        <button class="ui teal loading button">
                                            <i class="plus icon" /> Create
                                        </button>
                                    {:else}
                                        <button class="ui teal button">
                                            <i class="plus icon" /> Create
                                        </button>
                                    {/if}
                                </div>
                            </form>
                        </div>
                    </div>

                    <div class="twelve wide column">
                        <div class="">
                            <b> Latest Assignments </b>
                        </div>

                        <div class="">
                            <table class="ui inverted basic table">
                                <thead>
                                    <tr>
                                        <th> No. </th>
                                        <th> Name </th>
                                        <th> Hall </th>
                                        <th> Seat </th>
                                        <th> Date </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    {#each assignmentDetails.assignments as assignment}
                                        <tr>
                                            <td>
                                                {assignment.no}
                                            </td>
                                            <td>
                                                {assignment.user}
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
                            </table>
                        </div>
                    </div>
                </div>
            {/if}
        </div>
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

    .rightCol {
        border-right: 1px solid rgba(128, 128, 128, 0.452);
    }

    select {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
        height: 3.5em !important;
    }
</style>
