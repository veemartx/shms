<script>
    import { Router, Route, Link, navigate } from "svelte-navigator";
    import AdminProfile from "../routes/AdminProfile.svelte";
    import AdminStats from "../routes/AdminStats.svelte";
    import AdminStudents from "../routes/AdminStudents.svelte";
    import AdminAddStudent from "../routes/AdminAddStudent.svelte";
    import AdminStudent from "../routes/AdminStudent.svelte";
    import AdminManage from "../routes/AdminManage.svelte";
    import AdminAddHostel from "../routes/AdminAddHostel.svelte";
    import AdminHostel from "../routes/AdminHostel.svelte";
    import AdminRoom from "../routes/AdminRoom.svelte";
    import Assignments from "../routes/Assignments.svelte";
    import Transactions from "../routes/Transactions.svelte";
    import AdminMessages from "../routes/AdminMessages.svelte";
    import AdminLecturers from "./AdminLecturers.svelte";
    import AdminAddLecturer from "./AdminAddLecturer.svelte";
import AdminLecturer from "./AdminLecturer.svelte";

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
                    window.localStorage.removeItem("shmsLia");

                    navigate("/admin");
                } else {
                }
            });
    };

    let lia = window.localStorage.shmsLia;

    if (lia == undefined) {
        navigate("/admin/");
    }
</script>

<main>
    <Router>
        <div class="wrapper">
            <div class="ui grid">
                <div class="topCol sixteen wide column">
                    <div class="mainTitle">EXAM HALL ALLOCATION SYSTEM</div>
                </div>

                <div class="sideCol three wide column">
                    <div class="ui segmen" style="padding-left: 1em;">
                        <div class="compNav">
                            <Link to="/admin/dash">
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
                            <Link to="/admin/dash/students">
                                <i
                                    style="color: #11a5db;font-size:16px;"
                                    class="graduation cap icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Students
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/admin/dash/messages">
                                <i
                                    style="color: purple;font-size:16px;"
                                    class="mail icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Messages
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/admin/dash/manage">
                                <i
                                    style="color: teal;font-size:16px;"
                                    class="cogs icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Manage
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/admin/dash/lecturers">
                                <i
                                    style="color: teal;font-size:16px;"
                                    class="users icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Lecturers
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/admin/dash/courses">
                                <i
                                    style="color: crimson;font-size:16px;"
                                    class="file icon"
                                />
                                <div
                                    class=""
                                    style="color: aliceblue;padding-top: 5px;display:inline-block"
                                >
                                    Courses
                                </div>
                            </Link>
                        </div>

                        <div class="compNav">
                            <Link to="/admin/dash/profile">
                                <i
                                    style="color: orangered;font-size:16px;"
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
                        <AdminStats />
                    </Route>
                    <Route path="/profile">
                        <AdminProfile />
                    </Route>

                    <Route path="/manage/*">
                        <Route path="/">
                            <AdminManage />
                        </Route>

                        <Route path="/hostels/add">
                            <AdminAddHostel />
                        </Route>

                        <Route path="/hostels/*id" let:params>
                            <AdminHostel id={params.id} />
                        </Route>

                        <Route path="/room/*id" let:params>
                            <AdminRoom id={params.id} />
                        </Route>

                        <Route path="/assignments">
                            <Assignments />
                        </Route>

                        <Route path="/transactions">
                            <Transactions />
                        </Route>
                    </Route>

                    <Route path="/students/*">
                        <Route path="/">
                            <AdminStudents />
                        </Route>

                        <Route path="/add">
                            <AdminAddStudent />
                        </Route>

                        <Route path="/*id" let:params>
                            <AdminStudent id={params.id} />
                        </Route>
                    </Route>

                    <Route path="/lecturers/*">
                        <Route path="/">
                            <AdminLecturers />
                        </Route>

                        <Route path="/add">
                            <AdminAddLecturer />
                        </Route>

                        <Route path="/*id" let:params>
                            <AdminLecturer id={params.id} />
                        </Route>
                    </Route>

                    <Route path="/messages/*">
                        <Route path="/*">
                            <AdminMessages />
                        </Route>
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
