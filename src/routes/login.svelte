<script>
// @ts-nocheck

    import * as Realm from 'realm-web';
    const app = new Realm.App(import.meta.env.VITE_REALM_APP_ID)
    let email,password,message,emailError,passwordError;
    
    let passwordRegex = /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,}$/g
    let emailRegex = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/g;

    $: isVisible = false;
    const handleLogin = async() => {
        const credentials = Realm.Credentials.emailPassword(email?.toString(),password?.toString())
        return await app.logIn(credentials)
    }

    const handleClick = () => {
        if(!emailRegex.test(email)){
            emailError = "Enter Correct EMail"
        }
        if(!passwordRegex.test(password)){
            passwordError = "a password must be eight characters including one uppercase letter, one special character and alphanumeric characters"
        }

        if(!emailError && !passwordError){
            let promise = handleLogin()
            promise
            .then(user => {
                console.log(user);
                message = "User Logged In Successfully"
                setTimeout(() => {
                    window.location.pathname = '/'
                }, 3000);
            })
            .catch(err => {
                console.log(err)
                message = "Enter Correct Email/Passord Combination"
            })
        }
    }

    const handlePassword = (e) => {
        isVisible = !isVisible
        document.querySelector('#password').type = isVisible ? 'text' : 'password';
    }


</script>

<div class="login">
    <div class="message">
            Login
    </div>
    <div class="form">
        <input type="email" on:input="{() => emailError = ""}" placeholder="Enter Your EMail" bind:value={email} name="" id=""/>
        {#if emailError}
                <div class="error">
                    {emailError}
                </div>
        {/if}
        <div class="pass">
            <input type="password" on:input="{() => passwordError = ""}" placeholder="Enter Your Password" bind:value={password} name="" id="password">
            <div on:click={handlePassword}>
                {isVisible ? 'Hide' : 'Show'}
            </div>
        </div>
        {#if passwordError}
                <div class="error">
                    {passwordError}
                </div>
        {/if}
        <button type="submit" on:click="{handleClick}">Login</button>
    </div>
        <div class="signup">
            <h3>Don't Have an account</h3> 
            <a href="/signup">Create One</a>
        </div>
        <div class="forgot">
            <h3>
                Forgot Password
            </h3>
            <a href="/forgot">reset</a>
        </div>
    {#if message}
        <div class="message">
            {message}
        </div>
    {/if}
</div>

<style>
    .login {
        width: 100vw;
        height: 100vh;
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-top: 20vh;
    }
    .message {
        font-size: larger;
        font-weight: bold;
        margin: 3vh auto;
        transition: all 2s ease-in-out;
    }
    .signup, .forgot {
        display: flex;
        width: auto;
        align-items: center;
        justify-content: space-between;
    }

    .signup a, .forgot a {
        margin-left: 2vw;
        text-decoration: none;
        color: black;
        font-weight: bold;
    }
    .form {
        width: 60vw;
        padding: 2vh 3vw;
        border: 1px solid gray;
        border-radius: 30px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .pass {
        display: flex;
        align-items: center;
        width: 50vw;
        border-radius: 30px;
        padding: 0 15px;
        border: 1px solid gray;
    }
    input, button {
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

    .error {
        color: red;
        margin: 2vh auto;
    }
    .pass input {
        border:  none;
        margin: 0;
    }
    button {
        width: 20vw;
    }


</style>