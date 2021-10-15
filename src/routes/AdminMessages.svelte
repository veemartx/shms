<script>
    import { onMount } from "svelte";

    import { Router, Route, Link } from "svelte-navigator";
    import MessagesHomeComponent from "../components/MessagesHomeComponent.svelte";
    import MessagesComponent from "../components/MessagesComponent.svelte";

    import baseUrl from "../config/config";

    let chatUsers;

    const getChats = () => {
        fetch(baseUrl + "get-chat-users.php")
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                chatUsers = data;
            })
            .catch((e) => {
                console.log(data);
            });
    };

    onMount(() => {
        getChats();
    });
</script>

<main>
    <div class="wrapper">
        <Router>
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

                    &nbsp;&nbsp;&nbsp;&nbsp; Messages
                </div>

                <div class="sideCol five wide column">
                    {#if chatUsers != undefined}
                        {#each chatUsers as cu}
                            <a href={"/admin/dash/messages/" + cu.id}>
                                <div class="coversationCol">
                                    Chat With <b style="color: dodgerblue;"
                                        >{cu.name}</b
                                    >
                                </div>
                            </a>
                        {/each}
                    {/if}
                </div>
                <div class="eleven wide column">
                    <Route path="/">
                        <MessagesHomeComponent />
                    </Route>

                    <Route path="/*id" let:params>
                        <MessagesComponent id={params.id} />
                    </Route>
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

    .sideCol {
        border-right: 1px solid rgba(58, 58, 58, 0.459);
    }

    .coversationCol {
        height: 3.5em;
        padding: 1em;
        background: rgba(70, 70, 70, 0.336);
        margin-top: 3px;
        color: aliceblue;
    }
</style>
