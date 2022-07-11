<script>
    // @ts-nocheck
    
        import * as Realm from 'realm-web';
        const app = new Realm.App(import.meta.env.VITE_REALM_APP_ID)
        let email,password,message;

        const handleReset = async() => {
            return await app.emailPasswordAuth.sendResetPasswordEmail(email)
        }
    
        const handleClick = () => {
            let promise = handleReset()
            promise
            .then(user => {
                message = "Reset Mail is sent to you!!"
            })
            .catch(err => {
                message = "Enter Correct Email/Passord Combination"
            })
        }
    
    
    
    </script>
    
    <div class="login">
        <div class="message">
                Reset
        </div>
        <div class="form">
            <input type="email" placeholder="Enter Your EMail" bind:value={email} name="" id=""/>
            <button type="submit" on:click="{handleClick}">Reset Password</button>
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
    
        .form {
            width: 60vw;
            padding: 2vh 3vw;
            border: 1px solid gray;
            border-radius: 30px;
            display: flex;
            flex-direction: column;
            align-items: center;
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
        button {
            width: 20vw;
        }
    
    
    </style>