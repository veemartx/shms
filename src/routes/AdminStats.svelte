<script>
    import { onMount } from "svelte";
    import { v4 as uuidv4 } from "uuid";
    import { Router, Link } from "svelte-navigator";
    import baseUrl from "../config/config";

    let notificationType;

    let notificationMessage;

    let notificationTitle;

    let notificationStudent;

    let stats;

    let btnLoading = false;

    const clearForm = () => {
        let addUserForm = document.getElementById("postNotificationForm");

        // console.log(addUserForm);
        addUserForm.reset();
    };

    const postNotification = () => {
        let notification = {
            sender: window.localStorage.shmsLia,
            type: notificationType,
            title: notificationTitle,
            message: notificationMessage,
            recipient: notificationStudent,
            notificationId: uuidv4(),
        };

        // send this to the server

        btnLoading = true;

        fetch(baseUrl + "post-notification.php", {
            method: "POST",
            body: JSON.stringify(notification),
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

                getHomeStats();

                btnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                btnLoading = false;
            });

        console.log(notification);
    };

    const getHomeStats = () => {
        fetch(baseUrl + "get-admin-home-stats.php")
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                stats = data;
            })
            .catch((e) => {
                console.log(data);
            });
    };

    onMount(() => {
        getHomeStats();
    });
</script>

<main>
    <div class="wrapper">
        {#if stats != undefined}
            <div class="ui grid" style="background: rgb(34, 34, 34);">
                <div class="titleBar re sixteen wide column">
                    <!-- <span style="cursor: pointer;color:dodgerblue">
                    <i class="arrow left icon" /> Back
                </span> -->

                    <!-- &nbsp;&nbsp;&nbsp;&nbsp; -->
                    <i class="line graph icon" /> Quick Stats
                </div>

                <div class="sixteen wide column">
                    <div class="ui stackable  grid">
                        <div class="doubling four column row">
                            <div class="re column">
                                <Link to="/admin/dash/students/">
                                    <div class="statsPanel">
                                        <div class="iconContainer">
                                            <img
                                                src="/assets/icons/users.png"
                                                alt="products icon"
                                                width="32"
                                                height="32"
                                            />
                                        </div>

                                        <div class="descriptionContainer">
                                            <div class="figureContainer">
                                                {stats.allUsers}
                                            </div>
                                            <div class="description">Users</div>
                                        </div>
                                    </div>
                                </Link>
                            </div>

                            <div class="tea column">
                                <Link to="/admin/dash/manage/">
                                    <div class="statsPanel">
                                        <div class="iconContainer">
                                            <img
                                                src="/assets/icons/branches.png"
                                                alt="Out Of Stock icon"
                                                width="32"
                                                height="32"
                                            />
                                        </div>

                                        <div class="descriptionContainer">
                                            <div class="figureContainer">
                                                {stats.rooms}
                                            </div>
                                            <div class="description">Halls</div>
                                        </div>
                                    </div>
                                </Link>
                            </div>

                            <div class="brow column">
                                <Link to="/admin/dash/students">
                                    <div class="statsPanel">
                                        <div class="iconContainer">
                                            <img
                                                src="/assets/icons/users.png"
                                                alt="Short Expiry icon"
                                                width="32"
                                                height="32"
                                            />
                                        </div>

                                        <div class="descriptionContainer">
                                            <div class="figureContainer">
                                                {stats.usersNo}
                                            </div>
                                            <div class="description">
                                                Students
                                            </div>
                                        </div>
                                    </div>
                                </Link>
                            </div>

                            <div class="brow column">
                                <Link to="/admin/dash/admins">
                                    <div class="statsPanel">
                                        <div class="iconContainer">
                                            <img
                                                src="/assets/icons/admin.png"
                                                alt="Short Expiry icon"
                                                width="32"
                                                height="32"
                                            />
                                        </div>

                                        <div class="descriptionContainer">
                                            <div class="figureContainer">
                                                {stats.adminsNo}
                                            </div>
                                            <div class="description">
                                                Admins
                                            </div>
                                        </div>
                                    </div>
                                </Link>
                            </div>


                            <div class="brow column">
                                <Link to="/admin/dash/messages">
                                    <div class="statsPanel">
                                        <div class="iconContainer">
                                            <img
                                                src="/assets/icons/message.png"
                                                alt="Short Expiry icon"
                                                width="32"
                                                height="32"
                                            />
                                        </div>

                                        <div class="descriptionContainer">
                                            <div class="figureContainer">
                                                {stats.messages}
                                            </div>
                                            <div class="description">
                                                Messages
                                            </div>
                                        </div>
                                    </div>
                                </Link>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="seperatorCol sixteen wide column" />


                <div class="sixteen wide column">
                    <div class="notificationsTitle">Notifications</div>
                </div>

                <div class="seperatorCol sixteen wide column" />

                <div class="postNotification eight wide column ">
                    <div class="title">Post Notification</div>
                    <div class="">
                        <form
                            on:submit|preventDefault={postNotification}
                            class="ui form"
                            id="postNotificationForm"
                            style="padding-top: 1em;"
                        >
                            <div class="field">
                                <label for="title"> Title </label>

                                <input
                                    type="text"
                                    name="title"
                                    id="title"
                                    required
                                    placeholder="Title"
                                    bind:value={notificationTitle}
                                />
                            </div>

                            <div class="field">
                                <label for="type"> Type </label>

                                <select
                                    name="type"
                                    id="type"
                                    required
                                    bind:value={notificationType}
                                >
                                    <option value="">Select Type</option>
                                    <option value="All">All</option>
                                    <option value="Individual"
                                        >Individual</option
                                    >
                                    <!-- <option value="Group">Group</option> -->
                                </select>
                            </div>

                            {#if notificationType == "Individual"}
                                <div class="field">
                                    <label for="individual">
                                        Select Student
                                    </label>
                                    <select
                                        name="student"
                                        id="student"
                                        required
                                        bind:value={notificationStudent}
                                    >
                                        <option value="">Select Student</option>
                                        {#each stats.users as user}
                                            <option value={user}>{user}</option>
                                        {/each}
                                    </select>
                                </div>
                            {/if}

                            <div class="field">
                                <label for="message"> Message </label>

                                <textarea
                                    name="message"
                                    id="message"
                                    placeholder="Type Message"
                                    cols="30"
                                    rows="10"
                                    required
                                    bind:value={notificationMessage}
                                />
                            </div>

                            <div
                                class="field"
                                style="text-align: center;padding-top:1em;"
                            >
                                {#if btnLoading}
                                    <button class="ui red loading button">
                                        <i class="send icon" /> Send
                                    </button>
                                {:else}
                                    <button class="ui red button">
                                        <i class="send icon" /> Send
                                    </button>
                                {/if}
                            </div>
                        </form>
                    </div>
                </div>

                <div class="eight wide column">
                    <div class="title">Latest Notifications</div>

                    <div class="notifications">
                        {#each stats.latestNotifications as notification}
                            <div class="notification">
                                <div class="nTitle">
                                    {notification.title}
                                </div>
                                <div class="nMessage">
                                    {@html notification.message}
                                </div>

                                <div class="fromTo">
                                    <div class="to">
                                        To: {notification.recipient}
                                    </div>

                                    <div class="sentAt">
                                        {notification.sentAt}
                                    </div>
                                </div>
                            </div>
                        {/each}
                    </div>
                </div>
            </div>
        {/if}
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

    .title {
        font-weight: 600;
        text-decoration: underline;
    }
    .statsPanel {
        border: 1px solid rgba(128, 128, 128, 0.151);
        padding: 0.4em;
        border-radius: 4px;
        color: aliceblue;
        display: flex;
        flex-direction: row;
        margin-top: 1em;
        margin-bottom: 1em;
    }

    .statsPanel:hover {
        background: #31313194;
        border: 1px solid rgba(73, 73, 73, 0.137);
        color: orangered;
    }

    .iconContainer {
        text-align: right;
        flex: 1;
        padding-top: 1em;
    }

    .descriptionContainer {
        flex: 3;
    }

    .figureContainer {
        font-size: 20px;
        text-align: center;
        font-weight: 600;
        padding-top: 10px;
        /* color:rgb(179, 186, 192); */
    }

    .description {
        font-size: 14px;
        text-align: center;
        padding-top: 10px;
        color: rgb(179, 186, 192);
    }

    .seperatorCol {
        border-bottom: 1px solid rgba(128, 128, 128, 0.459);
    }

    .postNotification {
        border-right: 1px solid rgba(128, 128, 128, 0.459);
    }

    .notificationsTitle {
        font-weight: 600;
        font-size: large;
        text-align: center;
        padding-top: 1em;
    }

    .notification {
        margin-top: 1em;
        background: rgb(12, 58, 119);
        padding: 1em;
        border-radius: 5px;
    }

    .nTitle {
        font-weight: 600;
    }

    .nMessage {
        padding-top: 5px;

        font-style: italic;
    }

    .fromTo {
        padding-top: 1em;
        display: flex;
        color: rgba(255, 255, 255, 0.58);
    }

    .to {
        flex: 1;
    }

    .sentAt {
        flex: 1;
    }

    input {
        background: rgb(61, 61, 61) !important;
    }

    select {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
        height: 3.5em !important;
    }

    textarea {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
    }
</style>
