<script>
    import { onMount } from "svelte";
    import { navigate } from "svelte-navigator";
    import baseUrl from "../config/config";

    let fullname;

    let username;

    let email;

    let phone;

    let idNo;

    let btnLoading = false;

    let changePassBtnLoading = false;

    // change pass details
    let currentPass;

    let newPass;

    let confirmPass;

    const editUserProfile = () => {
        let userEditData = {
            userId: window.localStorage.shmsLiu,
            fullname: fullname,
            username: username,
            email: email,
            phone: phone,
            idNo: idNo,
        };

        btnLoading = true;

        fetch(baseUrl + "edit-user.php", {
            method: "POST",
            body: JSON.stringify(userEditData),
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

                changePassBtnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                changePassBtnLoading = false;
            });
    };

    const getUserProfile = () => {
        let user = {
            userId: window.localStorage.shmsLiu,
        };

        fetch(baseUrl + "get-user-profile.php", {
            method: "POST",
            body: JSON.stringify(user),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                // userDetails = data;

                fullname = data.fullname;

                username = data.username;

                email = data.email;

                phone = data.phone;

                idNo = data.idNo;

                console.log(data);
            })
            .catch((e) => {
                console.log(e);
            });
    };

    const handleChangePass = () => {
        changePassBtnLoading = true;

        if (newPass != confirmPass) {
            alert("Passwords Do Not Match");
        } else {
            let pass = {
                currentPass: currentPass,
                newPass: newPass,
                userId: window.localStorage.shmsLiu,
            };

            // send to the server

            fetch(baseUrl + "change-user-pass.php", {
                method: "POST",
                body: JSON.stringify(pass),
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

                    // clearForm();
                    getUserProfile();

                    changePassBtnLoading = false;
                })
                .catch((e) => {
                    console.log(e);

                    changePassBtnLoading = false;
                });
        }
    };

    onMount(() => {
        // get logged in user profile
        getUserProfile();
    });
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

                &nbsp;&nbsp;&nbsp;&nbsp; Profile
            </div>

            <!--  -->
            <div class="sixteen wide column">
                <form
                    class="ui form"
                    on:submit|preventDefault={editUserProfile}
                >
                    <div class="field">
                        <label for="fullname">Name</label>
                        <input
                            type="text"
                            name="name"
                            id="name"
                            placeholder="Fullname"
                            bind:value={fullname}
                        />
                    </div>

                    <div class="field">
                        <label for="username">Username</label>
                        <input
                            type="text"
                            name="username"
                            id="username"
                            placeholder="Username"
                            bind:value={username}
                        />
                    </div>

                    <div class="field">
                        <label for="email">Email</label>
                        <input
                            type="email"
                            name="email"
                            id="email"
                            placeholder="Email"
                            bind:value={email}
                        />
                    </div>

                    <div class="field">
                        <label for="phone">Phone</label>
                        <input
                            type="tel"
                            name="phone"
                            id="phone"
                            placeholder="Phone"
                            bind:value={phone}
                        />
                    </div>

                    <div class="field">
                        <label for="idNo">Id No</label>
                        <input
                            type="number"
                            name="idNo"
                            id="idNo"
                            placeholder="ID No"
                            bind:value={idNo}
                        />
                    </div>
                    <div
                        class="field"
                        style="padding-top: 2em;text-align:center;"
                    >
                        {#if btnLoading}
                            <button class="ui red loading button">
                                <i class="edit icon" /> Edit Profile
                            </button>
                        {:else}
                            <button class="ui red button">
                                <i class="edit icon" /> Edit Profile
                            </button>
                        {/if}
                    </div>
                </form>
            </div>
            <!--  -->

            <div class="separatorCol sixteen wide column">
                <b> Change Password </b>
            </div>

            <div class="sixteen wide column">
                <form
                    class="ui form"
                    id="changeAdminPassForm"
                    autocomplete="off"
                    on:submit|preventDefault={handleChangePass}
                >
                    <div class="field">
                        <label for="current password">Current Password</label>
                        <input
                            type="password"
                            name="currentPass"
                            id="currentPass"
                            autocomplete="off"
                            bind:value={currentPass}
                        />
                    </div>

                    <div class="field">
                        <label for="current password">New Password</label>
                        <input
                            type="password"
                            name="newPass"
                            id="newPass"
                            autocomplete="off"
                            bind:value={newPass}
                        />
                    </div>

                    <div class="field">
                        <label for="current password">Confirm Password</label>
                        <input
                            type="password"
                            name="confirmPass"
                            id="confirmPass"
                            autocomplete="off"
                            bind:value={confirmPass}
                        />
                    </div>

                    <div
                        class="field"
                        style="text-align: center;padding-top:2em"
                    >
                        {#if changePassBtnLoading}
                            <button class="ui blue loading button">
                                <i class="lock icon" /> Change Pass
                            </button>
                        {:else}
                            <button class="ui blue button">
                                <i class="lock icon" /> Change Pass
                            </button>
                        {/if}
                    </div>
                </form>
            </div>
            <!--  -->
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
        border-bottom: 1px solid rgba(128, 128, 128, 0.459);
    }
</style>
