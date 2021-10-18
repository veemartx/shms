<script>
    import { v4 as uuidv4 } from "uuid";
    import baseUrl from "../config/config";

    let name;

    let location;

    let units;

    let btnLoading = false;

    const clearForm = () => {
        let addHostelForm = document.getElementById("addHostelForm");

        addHostelForm.reset();
    };


    const handleAddHostel = () => {
        let hostel = {
            name: name,
            location: location,
            units: units,
            hostelId: uuidv4(),
        };

        

        //
        btnLoading = true;

        fetch(baseUrl + "add-hostel.php", {
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

                clearForm();

                btnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                btnLoading = false;
            });
    };
</script>

<main>
    <div class="wrapper">
        <div class="ui grid" style="background-color: rgb(34, 34, 34);;">
            <div class="titleBar re sixteen wide column">
                <span
                    on:click={() => {
                        window.history.back();
                    }}
                    style="cursor: pointer;color:dodgerblue"
                >
                    <i class="arrow left icon" /> Back
                </span>

                &nbsp;&nbsp;&nbsp;&nbsp; Add Hall
            </div>

            <div class="sixteen wide column">
                <div class="">
                    <form
                        on:submit|preventDefault={handleAddHostel}
                        class="ui form"
                        id="addHostelForm"
                    >
                        <div class="field">
                            <label for="name">Name</label>
                            <input
                                type="text"
                                name="name"
                                id="name"
                                required
                                placeholder="Hall Name"
                                bind:value={name}
                            />
                        </div>

                        <div class="field">
                            <label for="location">Location</label>
                            <input
                                type="text"
                                name="location"
                                id="location"
                                bind:value={location}
                                placeholder="Location"
                                required
                            />
                        </div>

                        <div class="field">
                            <label for="units">Seats</label>
                            <input
                                type="number"
                                name="units"
                                id="units"
                                bind:value={units}
                                placeholder="00"
                                required
                            />
                        </div>

                        <div
                            class="field"
                            style="text-align: center;padding-top:2em"
                        >
                            {#if btnLoading}
                                <button class="ui red loading button">
                                    <i class="plus icon" /> Add Hall
                                </button>
                            {:else}
                                <button class="ui red button">
                                    <i class="plus icon" /> Add Hall
                                </button>
                            {/if}
                        </div>
                    </form>
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

    input {
        background: rgb(61, 61, 61) !important;
    }
</style>
