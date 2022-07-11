<script>
    export let props;
</script>

<div class="cardContainer">
    <div class="title">
        <a href={props.url}>{props.title}</a>
    </div>
    <div class="userContainer">
        {#if props.user}
            <a href={props.user.profile_url} target="_blank" rel="noopener noreferrer">
                <img src={props.user.avatar_url} alt="User Avatar"/>
                <div class="username">{props.user?.username}</div>
            </a>
        {/if}

    </div>
</div>

<style>
    .cardContainer {
        height: 100%;
        font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 3vh 0;
    }

    .title a{
        text-decoration: none;
        color: black;
        font-size: large;
        margin: 2vh auto;
    }
    img {
        width: 50px;
        height: 50px;
        border-radius: 50%;
        margin: 0 2vw;
        border: 1px solid gray;
    }
    .userContainer a {
        text-decoration: none;
        color: black;
        display: flex;
        align-items: center;
        font-size: large;
        font-weight: bold;
        margin: 2vh auto;
    }
</style>