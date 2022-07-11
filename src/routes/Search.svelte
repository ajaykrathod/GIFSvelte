<script>
import { GiphyFetch } from "@giphy/js-fetch-api/dist";
import Card from "../lib/card.svelte";
import Library from "../lib/library.svelte";


    const gf = new GiphyFetch(import.meta.env.VITE_GIPHY_API_KEY)
    $: value = ""
    $: offset = 0;
    $: selected = {};
    $: listGIFURL = []
    $: listGIF = []
    
    async function search(offset,search) {
        const gifs = await gf.search(search,{
            limit:10,
            offset:offset
        })

        return gifs.data
    }

    const handlePromise = (promise) => {
        promise
            .then(results => {
                results.map((value,index) => {
                    listGIF.push(value)
                    listGIFURL.push(new URL(value.images.original.mp4).href)
                    listGIFURL = [...listGIFURL];
                    listGIF = [...listGIF];
                })
            })
            .catch(err => console.log(err))

    }
    window.onscroll = function(event) {
        if((window.innerHeight + window.scrollY) >= document.body.offsetHeight){
            offset = offset + 10;
            
            var promise = search(offset,value)
            handlePromise(promise)
        }
    }
    const handleChange = () => {
        offset = offset + 10;
        const promise = search(offset,value)
        listGIF = []
        listGIFURL = []
        listGIFURL = [...listGIFURL];
        listGIF = [...listGIF];
        handlePromise(promise)
    }

    const handleMouseEnter = (result) => {
        listGIF.map((value,index) => {
                if(new URL(value.images.original.mp4).href == result){
                    selected = value;
                    console.log(selected);
                }
        })
  }
</script>

<div class="searchContainer">
    <div class="searched">
        <input 
            type="search"
            on:input="{handleChange}"
            bind:value="{value}" 
            class="searchInput" 
            id=""
        />
    </div>
    <div class="renderer">
        {#each listGIFURL as result}
            {#if result}
                <div class="container">
                    <video on:mouseover="{() => handleMouseEnter(result)}" src={result} autoplay={true} loop muted/>
                    <div class="texts">
                            {#if selected != {}}
                                <Card props={selected}/>
                            {/if}
                            
                    </div>
                </div>
            {/if}
        {/each}
    </div>
</div>  
    <style>
        .renderer {
            display: flex;
            flex-direction: row;
            justify-content: center;
            flex-wrap: wrap;
        }

        .searched {
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        video {
            width: 20vw;
            margin: 2vh 1vw;
        }
        
        .container {
            height: auto;
            border-radius: 30px;
            margin: 2vh 2vw;
            border: 1px solid gray;
        }
        .container:hover video {
            visibility: none;
            opacity: 0;
            transition: all 3s ease-in-out;
        }
        .texts {
            position: absolute;
            width: 20vw;
            display: flex;
            border-radius: 30px;
            flex-direction: column;
            justify-content: center;
            visibility: none;
            opacity: 0;
        }
        .container:hover .texts {
            visibility: visible;
            opacity: 1;
            transition: all 3s ease-in-out;
            
        }
    
        .searchContainer {
            height: 100vh;
            width: 100vw;
        }

        .searchInput {
            border-radius: 20px;
            outline: none;
            border: 1px solid gray;
            line-height: 4vh;
            padding: 0 2vw;
            font-size: medium;
            margin: 2vh auto;
        }

        @media (max-width:750px){
            video {
                width: 37vw;
            }
            .texts {
                width: 37vw;
            }
        }

</style>