<script>
    import { onMount } from "svelte";
    import baseUrl from "../config/config";
    import { Router, Link, navigate } from "svelte-navigator";
    import { v4 } from "uuid";

    export let id;

    let name;

    let units;

    let location;

    let deleteLoading = false;

    let editLoading = false;

    let addRoomLoading = false;

    let roomCode;

    let rentPerMonth=0;

    let roomType='sc';

    let rooms;

    const getHostel = () => {
        let hid = {
            hostelId: id,
        };

        // send to the server
        fetch(baseUrl + "get-hostel.php", {
            method: "POST",
            body: JSON.stringify(hid),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                name = data.name;

                units = data.rooms;

                location = data.location;

                rooms = data.roomsArray;
            })
            .catch((e) => {
                console.log(e);
            });
    };

    onMount(() => {
        getHostel();
    });

    const deleteHostel = () => {
        if (confirm("Delete Hall? This action cannot be undone")) {
            // delete hostel
            let hostel = {
                id: id,
            };

            deleteLoading = true;

            // send this to server for deletion
            fetch(baseUrl + "delete-hostel.php", {
                method: "POST",
                body: JSON.stringify(hostel),
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

                    navigate("/admin/dash/manage");

                    deleteLoading = false;
                })
                .catch((e) => {
                    console.log(e);

                    deleteLoading = false;
                });
        }
    };

    const editHostel = () => {
        let hostel = {
            id: id,
            name: name,
            location: location,
            units: units,
        };

        editLoading = true;

        // send this to server for deletion
        fetch(baseUrl + "edit-hall.php", {
            method: "POST",
            body: JSON.stringify(hostel),
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

                editLoading = false;
            })
            .catch((e) => {
                console.log(e);

                editLoading = false;
            });
    };

    const addRoom = () => {
        let room = {
            hostelId: id,
            roomId: v4(),
            hostel: name,
            code: roomCode,
            rent: rentPerMonth,
            type: roomType,
        };

        // start loading the btn
        addRoomLoading = true;

        // send the data to the server
        fetch(baseUrl + "add-room.php", {
            method: "POST",
            body: JSON.stringify(room),
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

                    getHostel();
                } else {
                    toast.error(data.message);
                }

                addRoomLoading = false;
            })
            .catch((e) => {
                console.log(e);

                addRoomLoading = false;
            });

        console.log(room);
    };
</script>

<main>
    <div class="wrapper">
        <Router>
            <div class="ui grid" style="background-color:  rgb(34, 34, 34);;">
                <div class="titleBar re sixteen wide column">
                    <span
                        on:click={() => {
                            window.history.back();
                        }}
                        style="cursor: pointer;color:dodgerblue"
                    >
                        <i class="arrow left icon" /> Back
                    </span>

                    &nbsp;&nbsp;&nbsp;&nbsp; Hall
                </div>

                <div class="sixteen wide column">
                    <div class="">
                        <form
                            class="ui form"
                            on:submit|preventDefault={editHostel}
                        >
                            <div class="field">
                                <label for="name"> Name </label>

                                <input
                                    type="text"
                                    name="name"
                                    id="name"
                                    placeholder="Hostel Name"
                                    bind:value={name}
                                />
                            </div>

                            <div class="field">
                                <label for="name"> Location </label>

                                <input
                                    type="text"
                                    name="location"
                                    id="location"
                                    placeholder="Location"
                                    bind:value={location}
                                />
                            </div>

                            <div class="field">
                                <label for="name"> Units </label>

                                <input
                                    type="number"
                                    name="units"
                                    id="units"
                                    placeholder="Units"
                                    bind:value={units}
                                />
                            </div>

                            <div
                                class="field"
                                style="text-align: center;padding-top:1em;"
                            >
                                {#if editLoading}
                                    <button class="ui yellow loading button">
                                        <i class="edit icon" /> Edit
                                    </button>
                                {:else}
                                    <button class="ui yellow button">
                                        <i class="edit icon" /> Edit
                                    </button>
                                {/if}

                                {#if deleteLoading}
                                    <button
                                        on:click={deleteHostel}
                                        type="button"
                                        class="ui red loading button"
                                    >
                                        <i class="trash icon" /> Delete
                                    </button>
                                {:else}
                                    <button
                                        on:click={deleteHostel}
                                        type="button"
                                        class="ui red button"
                                    >
                                        <i class="trash icon" /> Delete
                                    </button>
                                {/if}
                            </div>
                        </form>
                    </div>
                </div>

                <div class="separatorCol sixteen wide column">
                    <h4>Seats</h4>
                </div>

                <div class="sideCol re four wide column">
                    <div class="">
                        <h5>
                            <i class="plus icon" /> ADD SEAT
                        </h5>

                        <br />
                    </div>
                    <div class="">
                        <form
                            class="ui form"
                            on:submit|preventDefault={addRoom}
                        >
                            <div class="field">
                                <label for="code"> Code </label>
                                <input
                                    type="text"
                                    name="roomCode"
                                    id="roomCode"
                                    placeholder="BS101"
                                    required
                                    bind:value={roomCode}
                                />
                            </div>

                            <!-- <div class="field">
                                <label for="roomType"> Room Type </label>
                                <select
                                    name="roomType"
                                    id="roomType"
                                    required
                                    bind:value={roomType}
                                >
                                    <option value="">Select Room Type</option>
                                    <option value="Single">Single</option>
                                    <option value="Studio">Studio</option>
                                    <option value="1 Bdrm">1 Bdrm</option>
                                    <option value="2 Bdrm">2 Bdrm</option>
                                </select>
                            </div> -->

                            <!-- <div class="field">
                                <label for="rentPerMonth"> Rent </label>
                                <input
                                    type="text"
                                    name="rentPerMonth"
                                    id="rentPerMonth"
                                    placeholder="1500.00"
                                    required
                                    bind:value={rentPerMonth}
                                />
                            </div> -->

                            <div
                                class="field"
                                style="text-align: center;padding-top:1em;"
                            >
                                {#if addRoomLoading}
                                    <button class="ui red loading button">
                                        <i class="add icon" /> Add
                                    </button>
                                {:else}
                                    <button class="ui red button">
                                        <i class="add icon" /> Add
                                    </button>
                                {/if}
                            </div>
                        </form>
                    </div>
                </div>

                <div class="yello twelve wide column">
                    {#if rooms != undefined}
                        <table class="ui striped unstackable table inverted">
                            <thead>
                                <tr>
                                    <th> No </th>
                                    <th> Code </th>
                                    <!-- <th> Rent </th> -->
                                    <!-- <th> Type </th> -->
                                    <th> Status </th>
                                    <th> Action </th>
                                </tr>
                            </thead>
                            <tbody>
                                {#each rooms as room}
                                    <tr>
                                        <td>
                                            {room.no}
                                        </td>
                                        <td>
                                            {room.code}
                                        </td>
                                        <!-- <td>
                                            {room.rent}
                                        </td> -->

                                        <!-- <td>
                                            {room.type}
                                        </td> -->

                                        <td>
                                            {#if room.status == "Vacant"}
                                                <span style="color:crimson">
                                                    {room.status}
                                                </span>
                                            {:else}
                                                <span style="color:green">
                                                    {room.status}
                                                </span>
                                            {/if}
                                        </td>
                                        <td>
                                            <a href={"../room/" + room.id}>
                                                <i class="eye icon" /> View</a
                                            >
                                        </td>
                                    </tr>
                                {/each}
                            </tbody>
                        </table>
                    {/if}
                </div>
            </div>
        </Router>
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

    .separatorCol {
        border-bottom: 1px solid rgba(240, 248, 255, 0.432);
    }

    .sideCol {
        border-right: 1px solid rgba(128, 128, 128, 0.315);
    }

    input {
        background: rgb(61, 61, 61) !important;
    }

    /* select {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
        height: 3.5em !important;
    } */
</style>
