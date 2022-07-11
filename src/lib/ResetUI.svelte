<script>
// @ts-nocheck
    export let code
    export let tokenID
    let password,confirmPassword;
    import * as Realm from 'realm-web'
    const app = new Realm.App(import.meta.env.VITE_REALM_APP_ID)
    $: isVisible = false;
    let regex = /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,}$/g
    $: isConfirmVisible = false
    let passwordError,confirmPasswordError;
    let message;


    const handleReset =  async() => {
        if(code && tokenID){
            const promise = await app.emailPasswordAuth.resetPassword(code,tokenID,password)
            return promise
        }
    }

   
   const handleClick = () => {
        if(!regex.test(password)){
            passwordError = "a password must be eight characters including one uppercase letter, one special character and alphanumeric characters"
        }
        if(!regex.test(confirmPassword)){
            confirmPasswordError = "a password must be eight characters including one uppercase letter, one special character and alphanumeric characters"
        }

        if(!passwordError && !confirmPasswordError){
            const promise = handleReset()
            promise
            .then(response => {
                message = "Password Reset Successfully"
            })
            .catch(err => {
                message = err.message
            })
        }
    }
        const handlePassword = (e) => {
            isVisible = !isVisible
            document.querySelector('#password').type = isVisible ? 'text' : 'password';
        }

        const handleConfirmPassword = (e) => {
            isConfirmVisible = !isConfirmVisible
            document.querySelector('#passwordconfirm').type = isConfirmVisible ? 'text' : 'password';
        }
</script>
    <div class="retryUI">
            <h2>Password Reset</h2>
        <div class="form">
            <div class="pass">
                <input  on:input="{() => passwordError = ""}" type="password" placeholder="Enter Your Password" bind:value={password} name="" id="password">
                <div on:click={handlePassword}>
                    {isVisible ? 'Hide' : 'Show'}
                </div>
            </div>
            <div class="error">
                {passwordError}
            </div>
            <div class="pass">
                <input on:input="{() => confirmPasswordError = ""}"  type="password" placeholder="Confirm Your Password" bind:value={confirmPassword} name="" id="passwordconfirm">
                <div on:click={handleConfirmPassword}>
                    {isConfirmVisible ? 'Hide' : 'Show'}
                </div>
            </div>
            <div class="error">
                {confirmPasswordError}
            </div>
            <button on:click="{handleClick}">Reset Passord</button>
        </div>
        {#if message}
            <div class="message">
                {message}
            </div>
        {/if}
    </div>
<style>
    .retryUI {
        display: flex;
        flex-direction: column;
        width: 100vw;
        height: 100vh;
        align-items: center;
    }

    .error {
        color: red;
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
            padding: 3px 3px;
            outline: none;
            border: 1px solid gray;
            font-size: medium;
        }

        .pass {
            display: flex;
            align-items: center;
            width: 50vw;
            margin: 2vh;
            border-radius: 30px;
            padding: 0 15px;
            border: 1px solid gray;
        }

        .pass input {
            border:  none;
            margin: 0;
        }
</style>