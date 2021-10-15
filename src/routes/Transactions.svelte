<script>
    import { onMount } from "svelte";
    import { v4 } from "uuid";
    import baseUrl from "../config/config";

    let transactionDetails;

    let btnLoading = false;

    // new transaction
    let user;

    let amount;

    let room;

    let code;

    const getTransactionDetails = () => {
        fetch(baseUrl + "get-transactions.php")
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                transactionDetails = data;
            })
            .catch((e) => {
                console.log(e);
            });
    };

    const addTransaction = () => {
        // get data

        let transaction = {
            user: user,
            room: room,
            code: code,
            amount: amount,
            id: v4(),
        };

        btnLoading = true;

        fetch(baseUrl + "add-transaction.php", {
            method: "POST",
            body: JSON.stringify(transaction),
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

                getTransactionDetails();

                btnLoading = false;
            })
            .catch((e) => {
                console.log(e);

                btnLoading = false;
            });
    };

    onMount(() => {
        getTransactionDetails();
    });
</script>

<main>
    <div class="wrapper">
        {#if transactionDetails != undefined}
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

                    &nbsp;&nbsp;&nbsp;&nbsp; Transactions
                </div>

                <div class="sideCol re four wide column">
                    <div
                        class=""
                        style="padding-top: 1em;border-bottom:1px solid gray;"
                    >
                        <i class="plus icon" /> Add Transaction
                        <br /> <br />
                    </div>

                    <div class="" style="padding-top: 1em;;">
                        <form
                            class="ui form"
                            on:submit|preventDefault={addTransaction}
                        >
                            <div class="field">
                                <label for="">User</label>
                                <select
                                    name="user"
                                    id="user"
                                    bind:value={user}
                                    required
                                >
                                    <option value="">Select User</option>
                                    {#each transactionDetails.users as user}
                                        <option value={user.name}
                                            >{user.name}</option
                                        >
                                    {/each}
                                </select>
                            </div>

                            <div class="field">
                                <label for="transactionCode"
                                    >transaction Code</label
                                >
                                <input
                                    type="text"
                                    name="code"
                                    id="code"
                                    placeholder="Transaction Code"
                                    required
                                    bind:value={code}
                                />
                            </div>

                            <div class="field">
                                <label for="amount">Transaction Amount</label>
                                <input
                                    type="number"
                                    name="amount"
                                    id="amount"
                                    placeholder="0.00"
                                    required
                                    bind:value={amount}
                                />
                            </div>

                            <div class="field">
                                <label for="amount">Room</label>
                                <input
                                    type="text"
                                    name="room"
                                    id="room"
                                    placeholder="D400"
                                    required
                                    bind:value={room}
                                />
                            </div>

                            <div
                                class="field"
                                style="padding-top: 1em;text-align:center;"
                            >
                                {#if btnLoading}
                                    <button class="ui red loading button">
                                        <i class="add icon" /> Add
                                    </button>
                                {:else}
                                    <button class="ui red button">
                                        <i class="add icon" /> Add
                                    </button>
                                {/if}
                                <br /> <br />
                            </div>
                        </form>
                    </div>
                </div>

                <div class="mainCol twelve wide column">
                    <div class="">
                        <table class="ui inverted basic table">
                            <thead>
                                <tr>
                                    <th>No</th>
                                    <th>Name</th>
                                    <th>Code</th>
                                    <th>Room</th>
                                    <th>Amount</th>
                                    <th> Date </th>
                                </tr>
                            </thead>
                            <tbody>
                                {#each transactionDetails.transactions as transaction}
                                    <tr>
                                        <td>{transaction.no}</td>
                                        <td>
                                            {transaction.user}
                                        </td>
                                        <td>
                                            {transaction.code}
                                        </td>
                                        <td>
                                            {transaction.room}
                                        </td>
                                        <td>
                                            {transaction.amount}
                                        </td>
                                        <td>
                                            {transaction.date}
                                        </td>
                                    </tr>
                                {/each}
                            </tbody>
                        </table>
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

    .sideCol {
        border-right: 1px solid rgb(165, 165, 165);
    }

    select {
        background: rgb(61, 61, 61) !important;
        color: aliceblue !important;
        height: 3.5em !important;
    }

    input {
        background: rgb(61, 61, 61) !important;
    }
</style>
