<script>
    import { onMount } from "svelte";
    import { navigate } from "svelte-navigator";
    import baseUrl from "../config/config";

    export let id;

    let fullname;

    let username;

    let email;

    let phone;

    let idNo;

    let gender;

    let btnLoading = false;

    let deleteBtnLoading = false;

    const getUserDetails = () => {
        let user = {
            userId: id,
        };

        fetch(baseUrl + "get-lecturer.php", {
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

                gender = data.gender;

                console.log(data);
            })
            .catch((e) => {
                console.log(e);
            });
    };

    const handleUserEdit = () => {
        let userEditData = {
            userId: id,
            fullname: fullname,
            username: username,
            phone: phone,
            email: email,
            idNo: idNo,
        };

        btnLoading = true;

        fetch(baseUrl + "edit-lecturer.php", {
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

                // clearForm();
                getUserDetails();

                btnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                btnLoading = false;
            });

        console.log(userEditData);
    };

    const handleUserDelete = () => {
        let userDeleteData = {
            userId: id,
        };

        console.log(userDeleteData);

        if (confirm("Delete This Lecturer? This Action Cannot Be Reversed")) {
            // fetch

            deleteBtnLoading = true;

            fetch(baseUrl + "delete-lecturer.php", {
                method: "POST",
                body: JSON.stringify(userDeleteData),
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

                    navigate("/admin/dash/lecturers");

                    deleteBtnLoading = false;
                })
                .catch((e) => {
                    console.log(e);

                    deleteBtnLoading = false;
                });
        }
    };
    // alert(id);
    onMount(() => {
        getUserDetails();
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

                &nbsp;&nbsp;&nbsp;&nbsp; Lecturer
            </div>

            <div class="sixteen wide column">
                <form class="ui form" on:submit|preventDefault={handleUserEdit}>
                    <div class="field">
                        <label for="fullname"> FullName </label>

                        <input
                            type="text"
                            name="fullname"
                            id="fullname"
                            bind:value={fullname}
                        />
                    </div>

                    <div class="field">
                        <label for="email">Email</label>

                        <input
                            type="email"
                            name="email"
                            id="email"
                            bind:value={email}
                        />
                    </div>

                    <div class="field">
                        <label for="phone">Phone</label>

                        <input
                            type="tel"
                            name="phone"
                            id="phone"
                            bind:value={phone}
                        />
                    </div>

                    <div
                        class="field"
                        style="text-align: center;padding-top:1em;"
                    >
                        {#if btnLoading}
                            <button class="ui orange loading button">
                                <i class="edit icon" /> Edit
                            </button>
                        {:else}
                            <button class="ui orange button">
                                <i class="edit icon" /> Edit
                            </button>
                        {/if}

                        {#if deleteBtnLoading}
                            <button
                                on:click={handleUserDelete}
                                type="button"
                                class="ui red loading button"
                            >
                                <i class="delete icon" /> Delete
                            </button>
                        {:else}
                            <button
                                on:click={handleUserDelete}
                                type="button"
                                class="ui red button"
                            >
                                <i class="delete icon" /> Delete
                            </button>
                        {/if}
                    </div>
                </form>
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
