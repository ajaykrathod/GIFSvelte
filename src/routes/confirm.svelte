<script>
    import * as Realm from 'realm-web'
    import ConfirmUi from '../lib/ConfirmUI.svelte';
    const app = new Realm.App(import.meta.env.VITE_REALM_APP_ID)
    let queryString = window.location.search
    let email;
    let urlParams = new URLSearchParams(queryString)
    let code = urlParams.get('token')
    let tokenID = urlParams.get('tokenId')
    let message;
    $: sendConfirmPasswordAgain = false;

    async function confirmUser(){
        if(code && tokenID){
            const promise = await app.emailPasswordAuth.confirmUser(code,tokenID)
            return promise
        }
    }

    $: error = false
    const promise = confirmUser()
    if(promise){
        promise.then(user =>{
            message = "User Confirmed"
            setTimeout(() => {
                window.location.pathname = '/login'
            }, 3000);
        })
        .catch(err => {
            message = err.message
            error = true
        })
    }

    const handleClick = async () => {
            sendConfirmPasswordAgain = true
    }
</script>
{#if sendConfirmPasswordAgain}
    <ConfirmUi/>
{:else}
    <div class="container">
        {#if message}
            <div class="message">
                {message}
            </div>
        {/if}
        {#if error}
            <button on:click="{handleClick}">
                Send Confirmation Email
            </button>
        {/if}
    </div>
{/if}

<style>

    .container {
        display: flex;
        flex-direction: column;
        font-size: large;
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
</style>