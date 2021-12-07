<script>
    import baseUrl from "../config/config";
    import { v4 as uuidv4 } from "uuid";

    let name;

    let email;

    let phone;

    let code;

    let yearSem;

    let btnLoading = false;

    let lecturer;

    const clearForm = () => {
        let addUserForm = document.getElementById("addUserForm");

        // console.log(addUserForm);
        addUserForm.reset();
    };

    const handleAddCourse = () => {
        let userData = {
            name: name,
            code: code,
            yearSem: yearSem,
            lecturer: lecturer,
            courseId: uuidv4(),
        };

        btnLoading = true;

        fetch(baseUrl + "add-course.php", {
            method: "POST",
            body: JSON.stringify(userData),
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

        console.log(userData);
    };
</script>

<main>
    <div class="wrapper">
        <div class="ui grid" style=" background: rgb(34, 34, 34);">
            <div class="titleBar re sixteen wide column">
                <span
                    on:click={() => {
                        window.history.back();
                    }}
                    style="cursor: pointer;color:dodgerblue"
                >
                    <i class="arrow left icon" /> Back
                </span>

                &nbsp;&nbsp;&nbsp;&nbsp; Add Lecturer
            </div>

            <div class="sixteen wide column">
                <div class="">
                    <form
                        class="ui form"
                        id="addUserForm"
                        on:submit|preventDefault={handleAddCourse}
                    >
                        <div class="two fields">
                            <div class="field">
                                <label for="fname"> Course Name </label>

                                <input
                                    type="text"
                                    name="name"
                                    id="name"
                                    placeholder="Computer Programming"
                                    required
                                    bind:value={name}
                                />
                            </div>

                            <div class="field">
                                <label for="fname"> Code </label>

                                <input
                                    type="text"
                                    name="code"
                                    id="code"
                                    placeholder="COMP 122"
                                    required
                                    bind:value={code}
                                />
                            </div>
                        </div>

                        <div class="two fields">
                            <div class="field">
                                <label for="fname"> Title </label>
                                <select
                                    name="title"
                                    id="title"
                                    bind:value={yearSem}
                                    required
                                >
                                    <option value="">Select Year/Sem</option>
                                    <option value="Y1S1">Y1S1</option>
                                    <option value="Y1S2">Y1S2</option>

                                    <option value="Y2S1">Y2S1</option>
                                    <option value="Y2S2">Y2S2</option>

                                    <option value="Y3S1">Y3S1</option>
                                    <option value="Y3S2">Y3S2</option>

                                    <option value="Y4S1">Y4S1</option>
                                    <option value="Y4S2">Y4S2</option>
                                </select>
                            </div>

                            <div class="field">
                                <label for="fname"> Email </label>

                                <input
                                    type="text"
                                    name="lecturer"
                                    id="lecturer"
                                    placeholder="Dr. Mwangi"
                                    bind:value={lecturer}
                                    required
                                />
                            </div>
                        </div>

                        <div
                            class="field"
                            style="text-align: center;padding-top:2em"
                        >
                            {#if btnLoading}
                                <button class="ui red loading button">
                                    <i class="plus icon" /> Add Course
                                </button>
                            {:else}
                                <button class="ui red button">
                                    <i class="plus icon" /> Add Course
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

    select {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
        height: 3.5em !important;
    }
</style>
