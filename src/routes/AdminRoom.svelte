<script>
    import { onMount } from "svelte";

    import baseUrl from "../config/config";

    export let id;

    let status;

    const getRoomDetails = () => {
        let room = {
            id: id,
        };

        fetch(baseUrl + "get-room.php", {
            method: "POST",
            body: JSON.stringify(room),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                status = data.status;

                hostels = data.hostels;

                rooms = data.rooms;

                users = data.users;

                
                console.log(data);
            })
            .catch((e) => {
                console.log(e);
            });
    };

    onMount(() => {
        getRoomDetails();
    });
</script>

<main>
    <div class="wrapper">
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

                &nbsp;&nbsp;&nbsp;&nbsp; Room
            </div>

            <div class="sixteen wide column">
                <div class="">
                    <form class="ui form">
                        <div class="field">
                            <label for="code"> Code </label>
                            <input type="text" name="code" id="code" />
                        </div>

                        <div class="field">
                            <label for="rent">Rent</label>
                            <input type="text" name="rent" id="rent" />
                        </div>

                        <div
                            class="field"
                            style="text-align: center;padding-top:2em;"
                        >
                            <button class="ui yellow button">
                                <i class="ui edit icon" /> Edit
                            </button>

                            <button class="ui red button">
                                <i class="ui trash icon" /> Delete
                            </button>
                        </div>
                    </form>
                </div>
            </div>

            <div class="separatorCol sixteen wide column">
                <div class="ui grid">
                    <div class="eight wide column">
                        <b> Assign/Reasign </b>
                    </div>

                    <div class="eight wide column">
                        <b> Latest Transactions </b>
                    </div>
                </div>
            </div>

            <div class="six wide column">
                <div class="">
                    {status}
                </div>

                <div class="ui form">
                    <div class="field">
                        <label for="">Status</label>
                        <select name="status" id="status">
                            <option value="">Select New Status</option>
                            <option value="Occupied">Occupied</option>
                            <option value="Vacant">Vacant</option>
                        </select>
                    </div>

                    <div
                        class="field"
                        style="text-align: center;padding-top:2em"
                    >
                        <button class="ui purple button">
                            <i class="send icon" /> Change
                        </button>
                    </div>
                </div>
            </div>

            <div class="ten wide column">
                <table class="ui inverted basic striped table">
                    <thead>
                        <tr>
                            <th> No </th>
                            <th> Name </th>
                            <th> Amount </th>
                            <th> Date </th>
                        </tr>
                    </thead>
                </table>
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

    input {
        background: rgb(61, 61, 61) !important;
    }

    .separatorCol {
        border-bottom: 1px solid gray;
    }

    select {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
        height: 3.5em !important;
    }
</style>
