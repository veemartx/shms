<script>
    import { onMount } from "svelte";
    import baseUrl from "../config/config";

    let transactions;

    const getMytransactions = () => {
        let liu = {
            liu: window.localStorage.shmsLiu,
        };

        fetch(baseUrl + "get-user-transactions.php", {
            method: "POST",
            body: JSON.stringify(liu),
            headers: {
                "Content-Type": "application/x-www-form-urlencoded",
            },
        })
            .then((res) => res.json())
            .then((data) => {
                console.log(data);

                transactions = data;
            })
            .catch((e) => {
                console.log(e);
            });
    };
    onMount(() => {
        getMytransactions();
    });
</script>

<main>
    <div class="wrapper">
        <div class="ui grid" style="background: rgb(34, 34, 34);;">
            <div class="titleBar re sixteen wide column">
                <span style="cursor: pointer;color:dodgerblue">
                    <i class="arrow left icon" /> Back
                </span>

                &nbsp;&nbsp;&nbsp;&nbsp; My transactions
            </div>

            <div class="sixteen wide column">
                <div class="">
                    <table class="ui inverted striped basic table">
                        <thead>
                            <tr>
                                <th>No</th>
                                <th>Code</th>
                                <th>Room</th>
                                <th>Amount</th>
                                <th>Date</th>
                            </tr>
                        </thead>
                        {#if transactions != undefined}
                            <tbody>
                                {#each transactions as transaction}
                                    <tr>
                                        <td>
                                            {transaction.no}
                                        </td>
                                        <td>
                                            {transaction.hostel}
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
                        {/if}
                    </table>
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
</style>
