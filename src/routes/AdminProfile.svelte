<script>
    import { onMount } from "svelte";
    import baseUrl from "../config/config";

    let fullname;

    let username;

    let email;

    let phone;

    let btnLoading = false;

    const editAdminProfile = () => {
        let adminEditData = {
            adminId: window.localStorage.shmsLia,
            fullname: fullname,
            username: username,
            email: email,
            phone: phone,
        };

        btnLoading = true;

        fetch(baseUrl + "edit-admin.php", {
            method: "POST",
            body: JSON.stringify(adminEditData),
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
                getAdminProfile();

                btnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                btnLoading = false;
            });
    };

    const getAdminProfile = () => {
        let admin = {
            adminId: window.localStorage.shmsLia,
        };

        fetch(baseUrl + "get-admin-profile.php", {
            method: "POST",
            body: JSON.stringify(admin),
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

                console.log(data);
            })
            .catch((e) => {
                console.log(e);
            });
    };

    onMount(() => {
        // get logged in admin profile
        getAdminProfile();
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
                    on:submit|preventDefault={editAdminProfile}
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
                >
                    <div class="field">
                        <label for="current password">Current Password</label>
                        <input
                            type="password"
                            name="currentPass"
                            id="currentPass"
                        />
                    </div>

                    <div class="field">
                        <label for="current password">New Password</label>
                        <input type="password" name="newPass" id="newPass" />
                    </div>

                    <div class="field">
                        <label for="current password">Confirm Password</label>
                        <input
                            type="password"
                            name="confirmPass"
                            id="confirmPass"
                        />
                    </div>

                    <div
                        class="field"
                        style="text-align: center;padding-top:2em"
                    >
                        <button class="ui blue button">
                            <i class="lock icon" /> Change Pass
                        </button>
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
