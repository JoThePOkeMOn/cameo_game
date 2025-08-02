<script>
    // import { createEventDispatcher } from "svelte";
    let {celeb,onselect} = $props()
    // const dispatch = createEventDispatcher();
    // const select =()=>{
    //     onselect()
    // }
    // function handleClick(){
    //     select()
    //     // dispatch('select')
    // }
    
  
</script>

<div class="card-container">
    <button class="card" onclick={()=>{onselect}} style="background-image:url({celeb.image})">
        <div class="details">
            <h1><a target="_blank" href="https://cameo-explorer.netlify.app/celebs/${celeb.id}">{celeb.nameL}</a></h1>
            <p class="type ">{celeb.type}</p>
        </div>
    </button>
</div>

<style>
    .card-container{
        width: 100%;
        height: 100%;
        position: relative;
    }
.card {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: 50% 50% no-repeat;
    background-position: 50% 0;
    background-size: cover;
    overflow: hidden;
    padding: 0;
    text-align: left;
}
     .details{
       position: absolute;
       width: 100%;
       bottom: 0;
       padding: 1em 0.5em 0.5em 0.5em;
       color: white;
       background: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.6) 35%, rgba(0, 0, 0, 0.9));
    }
    
    h1{
        margin: 0 0 0.2em 0;
    }
    .type{
        margin: 0;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    @media (min-width: 640px){
        .card{
            height: 0;
            padding: 0 0 100% 0;

        }
    }
</style>