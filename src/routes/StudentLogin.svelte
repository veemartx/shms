<script>
    import { Router, Link, navigate } from "svelte-navigator";
    import { onMount } from "svelte";
    import baseUrl from "../config/config";

    let btnLoading = false;

    let username;

    let password;

    const handleUserLogin = () => {
        let userLoginData = {
            username: username,
            password: password,
        };
        // start loading the btn
        btnLoading = true;

        fetch(baseUrl + "user-login.php", {
            method: "POST",
            body: JSON.stringify(userLoginData),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                btnLoading = false;

                if (data.success == true) {
                    window.localStorage.shmsLiu = data.tkn;

                    navigate("/student/dash");

                    toast.success(data.message);
                } else {
                    toast.error(data.message);
                }
                console.log(data);
            })
            .catch((err) => {
                btnLoading = false;
                console.log(err);
            });
    };

    // check if the user is logged in
    onMount(() => {
        // check if the user is logged in

        let lia = window.localStorage.shmsLiu;

        if (lia != undefined) {
            navigate("/student/dash");
        }
    });
</script>

<main>
    <div class="wrapper">
        <Router>
            <div class="ui centered grid">
                <div class="re sixteen wide column">
                    <div class="mainNav">
                        <Link to="/">
                            <i
                                style="color: aliceblue;"
                                class="home icon"
                            /><span class="link"> Home </span>
                        </Link>

                        <Link to="/admin">
                            <i class="user icon" style="color: aliceblue;" />
                            <span class="link"> Admin </span>
                        </Link>
                    </div>
                </div>

                <div class="sixteen wide column" />

                <div class="brow six wide column">
                    <div class="ui segment" style="margin-top: 3em;">
                        <div class="loginTitle">
                            <i class="user icon" /> <br />
                            <div class="" style="padding-top: 10px;">
                                User Login
                            </div>
                        </div>

                        <div class="" style="padding-top: 1em;">
                            <form
                                class="ui form"
                                on:submit|preventDefault={handleUserLogin}
                            >
                                <div class="field">
                                    <label for="username"> Username </label>
                                    <input
                                        type="text"
                                        name="username"
                                        id="username"
                                        placeholder="Your Username"
                                        bind:value={username}
                                    />
                                </div>

                                <div class="field">
                                    <label for="password"> Password </label>
                                    <input
                                        type="password"
                                        name="password"
                                        id="password"
                                        autocomplete="off"
                                        placeholder="Your Password"
                                        bind:value={password}
                                    />
                                </div>

                                <div
                                    class="field"
                                    style="padding: 2em;text-align:center;"
                                >
                                    {#if btnLoading}
                                        <button class="ui red loading button">
                                            <i class="lock icon" /> LOGIN
                                        </button>
                                    {:else}
                                        <button class="ui red basi button">
                                            <i class="lock icon" /> LOGIN
                                        </button>
                                    {/if}
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </Router>
    </div>
</main>

<style>
    .wrapper {
        max-width: 85em;
        margin: 2em auto;
    }

    .mainNav {
        text-align: right;
    }

    .link {
        color: aliceblue;
        margin-inline: 1em;
        font-weight: 400;
        font-size: 15px;
    }

    .loginTitle {
        font-weight: 100;
        text-align: center;
        font-size: 20px;
        border-bottom: 1px solid rgb(125, 125, 126);
        padding-bottom: 10px;
    }
</style>
