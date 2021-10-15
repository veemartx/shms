<script>
    import { onMount } from "svelte";
    import baseUrl from "../config/config";
    import { v4 as uuidv4 } from "uuid";

    let messages;

    let btnLoading = false;

    let notificationMessage;

    const getMessages = () => {
        let liu = {
            liu: window.localStorage.shmsLiu,
        };

        fetch(baseUrl + "get-user-messages.php", {
            method: "POST",
            body: JSON.stringify(liu),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                messages = data;
            })
            .catch((e) => {
                console.log(e);
            });
    };

    onMount(() => {
        getMessages();
    });

    const clearForm = () => {
        let sendMessageForm = document.getElementById("sendMessageForm");

        // console.log(addUserForm);
        sendMessageForm.reset();
    };

    const postNotification = () => {
        let notification = {
            sender: window.localStorage.shmsLiu,
            type: "Individual",
            title: "--",
            message: notificationMessage,
            recipient: "Admin",
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

                getMessages();

                btnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                btnLoading = false;
            });

        console.log(notification);
    };
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

                &nbsp;&nbsp;&nbsp;&nbsp; My Messages (Chatting With Admin)
            </div>

            <div class="sixteen wide column">
                {#if messages != undefined}
                    <div class="messagesBox">
                        {#each messages.reverse() as message}
                            {#if message.type == "sent"}
                                <div class="sent">
                                    <div class="title">
                                        {message.title}
                                    </div>

                                    <div class="message">
                                        {@html message.message}
                                    </div>
                                </div>

                                <div class="separator" />
                            {:else}
                                <div class="received">
                                    <div class="title">
                                        {message.title}
                                    </div>

                                    <div class="message">
                                        {@html message.message}
                                    </div>
                                </div>
                                <div class="separator" />
                            {/if}
                        {/each}
                    </div>
                {/if}

                <div class="messageForm">
                    <form
                        class="ui form"
                        id="sendMessageForm"
                        on:submit|preventDefault={postNotification}
                    >
                        <div class="two fields">
                            <div class="field" style="width: 96%;">
                                <textarea
                                    name="message"
                                    id="message"
                                    cols="30"
                                    rows="2"
                                    placeholder="Type Message"
                                    bind:value={notificationMessage}
                                />
                            </div>
                            <div
                                class="field"
                                style="padding-top: .3em;width:10%;"
                            >
                                {#if btnLoading}
                                    <button class="ui blue loading button">
                                        <i class="send icon" /> Send
                                    </button>
                                {:else}
                                    <button class="ui blue button">
                                        <i class="send icon" /> Send
                                    </button>
                                {/if}
                            </div>
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

    .messagesBox {
        height: 30em;
        overflow: auto;
        background-color: rgba(29, 29, 29, 0.267);
        padding: 1em;
        margin-bottom: 1em;
    }

    textarea {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
    }

    .sent {
        background-color: rgb(17, 17, 73);
        margin: 1em;
        border-radius: 5px;
        /* text-align: right; */
        max-width: 30em;
        float: right;
        padding: 1em;
    }

    .received {
        background-color: rgb(51, 51, 104);
        margin: 1em;
        border-radius: 5px;
        max-width: 30em;
        padding: 1em;
    }

    .title {
        font-weight: 600;
        padding-bottom: 5px;
    }
    .separator {
        clear: both;
    }
</style>
