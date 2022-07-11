<script>
    import ResetUi from '../lib/ResetUI.svelte';
    
    let queryString = window.location.search
    let urlParams = new URLSearchParams(queryString)
    let code = urlParams.get('token')
    let tokenID = urlParams.get('tokenId')

</script>
    <ResetUi 
        code={code}
        tokenID={tokenID}
    />
