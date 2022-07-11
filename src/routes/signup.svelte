<script>
    // @ts-nocheck
        import * as Realm from 'realm-web';
        let email,password,firstName,lastName,username,message,firstNameError,lastnameError,usernameError,emailError,passwordError;
        $: isVisible = false;

        let passwordRegex = /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).{8,}$/g
        let emailRegex = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/g;

        const app = new Realm.App(import.meta.env.VITE_REALM_APP_ID)
        const handleSignup = async () => {
            return await app.emailPasswordAuth.registerUser(email,password)     
        }
    
        const handlePassword = (e) => {
            isVisible = !isVisible
            document.querySelector('#password').type = isVisible ? 'text' : 'password';
        }

        const handleClick = () => {
            if(!emailRegex.test(email)){
                emailError = "Enter Correct EMail"
            }
            if(!passwordRegex.test(password)){
                passwordError = "a password must be eight characters including one uppercase letter, one special character and alphanumeric characters"
            }
            if(!firstName){
                firstNameError = "Enter First Name"
            }
            if(!lastName){
                lastnameError = "Enter Last Name"
            }
            if(!username){
                usernameError = "Enter Username"
            }

            if(!firstNameError && !lastnameError && !usernameError && !emailError && !passwordError){
                let promise = handleSignup()
                promise.then(user => {
                    message = "Confirmation EMail On The Way to your Inbox"
                })
                .catch(err => {
                    message = err.message
                })
            }
        }
    
    </script>
    
    <div class="login">
        <div class="message">
                SignUp
        </div>
        <div class="form">
            <div class="name">
                <div class="firstName">
                    <input type="text" on:input="{() => firstNameError = ""}" placeholder="Enter Your First Name" bind:value="{firstName}" name="firstName" id="">
                    {#if firstNameError}
                        <div class="error">
                            {firstNameError}
                        </div>
                    {/if}
                </div>
                <div class="lastName">
                    <input type="text" on:input="{() => lastnameError = ""}" placeholder="Enter Your Last Name" bind:value="{lastName}" name="lastName" id="">
                    {#if lastnameError}
                        <div class="error">
                            {lastnameError}
                        </div>
                    {/if}
                </div>
            </div>
            <input type="text" on:input="{() => usernameError = ""}" placeholder="Enter Your Username" bind:value="{username}" name="Username" id="">
            {#if usernameError}
                <div class="error">
                    {usernameError}
                </div>
            {/if}
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
            <button type="submit" on:click="{handleClick}">SignUp</button>
        </div>
        <div class="log">
            <h3>Have an account</h3> 
            <a href="/login">Login</a>
        </div>
        <div class="forgot">
            <h3>
                Forgot Password
            </h3>
            <a href="/forgot">reset</a>
        </div>
        {#if message}
            {message}
        {/if}
    </div>
    
    <style>
        .login {
            width: 100vw;
            height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 0vh;
        }
        .message {
            font-size: larger;
            font-weight: bold;
            margin-bottom: 3vh;
        }

        .error {
            color: red;
        }

        .log, .forgot {
            display: flex;
            width: auto;
            align-items: center;
            justify-content: space-between;
        }

        .log a, .forgot a {
            margin-left: 2vw;
            text-decoration: none;
            color: gray;
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

        .name {
            display: flex;
        }
        .pass {
            display: flex;
            align-items: center;
            width: 50vw;
            margin-top: 2vh;
            border-radius: 30px;
            padding-right: 15px;
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
        
        .name input {
            width: 22vw;
        }
        .pass input {
            border:  none;
            margin: 0;
        }
        button {
            width: 20vw;
        }
    
    
    </style>