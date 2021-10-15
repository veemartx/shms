<script>
    import { Router, Route, Link, navigate } from "svelte-navigator";
    import UserProfile from "../routes/UserProfile.svelte";
    import UserStats from "../routes/UserStats.svelte";
    import UserMessages from "../routes/UserMessages.svelte";
    import UserAssignments from "../routes/UserAssignments.svelte";
    import UserTransactions from "../routes/UserTransactions.svelte";

    const logout = () => {
        // confime
        toast
            .yesNoDialog({
                title: "Logout?",
                text: "Are You Sure You Want To Log Out?",
                textConfirm: "Yes",
                textCancel: "No",
                showCancel: true, // true or false
                type: "question", // 'success', 'danger', 'warning', 'info' or 'question'
                dark: false, // Show dark theme? 'true' or 'false'
            })
            .then(function (value) {
                if (value) {
                    // console.log("You clicked Confirm");
                    window.localStorage.removeItem("shmsLiu");

                    navigate("/student");
                } else {
                }
            });
    };

    let lia = window.localStorage.shmsLiu;

    if (lia == undefined) {
        navigate("/student/");
    }
</script>

<main>
    <Router>
        <div class="wrapper">
            <div class="ui grid">
                <div class="topCol sixteen wide column">
                    <div class="mainTitle">AJAAZ HOSTELS</div>
                </div>

                <div class="sideCol three wide column">
                    <div class="ui segmen" style="padding-left: 1em;">
                        <div class="compNav">
                            <Link to="/student/dash">
                                <i
                                    style="color: #85ed85;font-size:16px;"
                                    class="home icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Home
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/student/dash/transactions">
                                <i
                                    style="color: #11a5db;font-size:16px;"
                                    class=" exchange icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Transactions
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/student/dash/assignments">
                                <i
                                    style="color: teal;font-size:16px;"
                                    class="download icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Assignments
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/student/dash/messages">
                                <i
                                    style="color: orangered;font-size:16px;"
                                    class="mail icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    My Messages
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/student/dash/profile">
                                <i
                                    style="color: purple;font-size:16px;"
                                    class="user icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Profile
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <div
                                style="cursor: pointer;"
                                on:click={() => {
                                    logout();
                                }}
                            >
                                <i
                                    style="color: #e3ac14;font-size:16px;"
                                    class="power icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Logout
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="mainCol thirteen wide column">
                    <Route path="/">
                        <UserStats />
                    </Route>
                    <Route path="/profile">
                        <UserProfile />
                    </Route>

                    <Route path="/messages">
                        <UserMessages />
                    </Route>

                    <Route path="/assignments">
                        <UserAssignments />
                    </Route>

                    <Route path="/transactions">
                        <UserTransactions />
                    </Route>
                </div>
            </div>
        </div>
    </Router>
</main>

<style>
    .wrapper {
        max-width: 85em;
        margin: auto;
    }

    .mainTitle {
        font-weight: 600;
        font-size: 17px;
        text-align: left;
    }

    .topCol {
        background-color: rgb(34, 34, 34);
        border-bottom: 1px solid rgba(128, 128, 128, 0.377);
    }

    .sideCol {
        background-color: rgb(34, 34, 34);
    }

    .compNav {
        border-bottom: 1px solid gray;
        margin-top: 1em;
        margin-bottom: 1em;
        padding-bottom: 1em;
    }
</style>
