<script>
    let email;
    import * as Realm from 'realm-web'
    const app = new Realm.App(import.meta.env.VITE_REALM_APP_ID)
    $: error = false;
    let message;
    const handleConfirmation =  async() => {
        return await app.emailPasswordAuth.resendConfirmationEmail(email)
   }

   const promise = handleConfirmation()

   const handleClick = () => {
        promise
        .then(response => {
            message = "Confirmation is on it's way"
        })
        .catch(err => {
            error = true
            message = err.message
        })
   }
</script>
{#if !message}
    <div class="retryUI">
            <h2>Resend Email Confirmation</h2>
        <div class="form">
            <input type="email" placeholder="Enter Your EMail" bind:value="{email}" id="">
            <button on:click="{handleClick}">Send Confirmation EMail</button>
        </div>
        {#if message}
        <div class="message">
            {message}
        </div>
    {/if}
    </div>
{:else}
    {message}
{/if}
<style>
    .retryUI {
        display: flex;
        flex-direction: column;
        width: 100vw;
        height: 100vh;
        align-items: center;
    }
    .form {
        display: flex;
        flex-direction: column;
        border-radius: 30px;
        border: 1px solid gray;
        padding: 2vh 3vw;
    }
    button {
        width: 25vw;
        margin: 2vh auto;
        background: none;
        border: 1px solid gray;
        padding: 5px 15px;
        line-height: 3vh;
        border-radius: 20px;
    }

    input {
            width: 50vw;
            border-radius: 20px;
            line-height: 4vh;
            padding: 3px 10px;
            background: none;
            outline: none;
            margin: 2vh;
            border: 1px solid gray;
            font-size: medium;
        }


</style>