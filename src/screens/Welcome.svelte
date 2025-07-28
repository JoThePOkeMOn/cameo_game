<script>
    // let {selection} =$props();
    // import { createEventDispatcher } from "svelte";
    // const dispatch = createEventDispatcher()
    let {onSelection} = $props();
    let selected = $state(false);
    let categories = [
        {slug:'actors',label:'Actors'},
        {slug:'athletes',label:'Athletes'},
        {slug:'comedians',label:'Comedians'},
        {slug:'creators',label:'Creators'},
        {slug:'musicians',label:'Musicians'},
        {slug:'reality-tv',label:'Reality TV'},
    ]
// const selection =(category)=>{
//     selected = true;
//     dispatch('selection',category)
// }
    // const handleSelection = (category)=>{
    //     // selected =true
    //     onselect = category;
    // }
    const select = (category)=>{
        selected = true
        onSelection(category)
    }
    function handleClick(category){
        select(category)
    }
</script>

<main>
    <header><h1>
		CameoP<span class="logo">a</span>rison
	</h1> 

	<p class="svelte-1fhxxll">
		On <a href="https://cameo.com">cameo.com</a>, you can buy personalised video clips from everyone from Lindsay Lohan to Ice T.
	</p> 

	<p class="svelte-1fhxxll">But who commands the highest price?</p></header>
    <p>Select a category to play</p>
    <div class="categories">
        {#each categories as category}
            <button disabled={selected} onclick={()=>{handleClick(category.slug)}}>{category.label}</button>
        {/each}
    </div>
    .
</main>

<style>
h1{
    color: #ff3e00;
    text-transform: uppercase;
    font-size: min(12vw, 4em);
    font-weight: 100;
}
.logo {
    display: inline-block;
    background: url(../assets/compare.svg) 50% 50% no-repeat;
    background-size: 100% 100%;
    width: 0.8em;
    top: 0.05em;
    transform: scale(1.4);
    left: 0.02em;
    text-indent: -9999px;

}
</style>