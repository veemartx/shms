<script>
    import baseUrl from "../config/config";
    import { v4 as uuidv4 } from "uuid";

    let fname = "";
    let lname = "";

    $: fullname = fname + " " + lname;

    let email;

    let phone;

    let username;

    let idNo;

    let gender;

    $: username = fname.toLowerCase() + lname.toLowerCase();

    let btnLoading = false;

    const clearForm = () => {
        let addUserForm = document.getElementById("addUserForm");

        // console.log(addUserForm);
        addUserForm.reset();
    };


    const handleAddUser = () => {
        let userData = {
            name: fullname,
            email: email,
            username: username,
            phone: phone,
            idNo: idNo,
            gender: gender,
            userId: uuidv4(),
        };

        btnLoading = true;

        fetch(baseUrl + "add-user.php", {
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

                &nbsp;&nbsp;&nbsp;&nbsp; Add Student
            </div>

            <div class="sixteen wide column">
                <div class="">
                    <form
                        class="ui form"
                        id="addUserForm"
                        on:submit|preventDefault={handleAddUser}
                    >
                        <div class="two fields">
                            <div class="field">
                                <label for="fname"> First Name </label>

                                <input
                                    type="text"
                                    name="fname"
                                    id="fname"
                                    placeholder="John"
                                    required
                                    bind:value={fname}
                                />
                            </div>

                            <div class="field">
                                <label for="fname"> Last Name </label>

                                <input
                                    type="text"
                                    name="lname"
                                    id="lname"
                                    placeholder="Doe"
                                    required
                                    bind:value={lname}
                                />
                            </div>
                        </div>

                        <div class="two fields">
                            <div class="field">
                                <label for="fname"> Username </label>

                                <input
                                    type="text"
                                    name="username"
                                    id="username"
                                    placeholder="Username"
                                    bind:value={username}
                                    readonly
                                />
                            </div>

                            <div class="field">
                                <label for="fname"> Id No </label>

                                <input
                                    type="text"
                                    name="idNo"
                                    id="idNo"
                                    bind:value={idNo}
                                    required
                                    placeholder="00000000"
                                />
                            </div>
                        </div>

                        <div class="two fields">
                            <div class="field">
                                <label for="fname"> Email </label>

                                <input
                                    type="email"
                                    name="email"
                                    id="email"
                                    placeholder="abc@example.com"
                                    bind:value={email}
                                    required
                                />
                            </div>

                            <div class="field">
                                <label for="phone"> Phone </label>

                                <input
                                    type="tel"
                                    name="phone"
                                    id="phone"
                                    bind:value={phone}
                                    placeholder="07 XX XXX XXX"
                                />
                            </div>
                        </div>

                        <div class="field">
                            <label for="gender">Gender</label>
                            <select
                                name="gender"
                                id="gender"
                                required
                                bind:value={gender}
                            >
                                <option value="">Select Gender</option>
                                <option value="Female">Female</option>
                                <option value="Male">Male</option>
                                <option value="Other">Other</option>
                            </select>
                        </div>

                        <div
                            class="field"
                            style="text-align: center;padding-top:2em"
                        >
                            {#if btnLoading}
                                <button class="ui red loading button">
                                    <i class="plus icon" /> Add User
                                </button>
                            {:else}
                                <button class="ui red button">
                                    <i class="plus icon" /> Add User
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
