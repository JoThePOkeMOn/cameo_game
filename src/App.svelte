<script> 
  import { onMount } from "svelte";
  import { select } from "./select.js";
  import Welcome from "./screens/Welcome.svelte"
  import Playing from "./screens/Playing.svelte";

  let states = $state("Welcome");
  let celebs_promise;
  let selection = $state();
  
  const start = async (category)=>{
    // console.log(category)
    const {celebs,lookup} = await celebs_promise
    // console.log(celebs)
    // console.log(lookup)
    selection = select(celebs,lookup,category)
    states ="playing"
  }
  const load_celebs =async ()=>{
    const res = await fetch("https://cameo-explorer.netlify.app/celebs.json")
    // console.log(res)
    const data = await res.json();
    // console.log(data)

    const lookup = new Map();
    data.forEach(c =>{
      lookup.set(c.id,c)
    })

    const subset = new Set();
    data.forEach(c =>{
      if(c.reviews >= 50){
        subset.add(c)
        if(c.similar.length !== 0){
        c.similar.forEach(id =>{
          subset.add(lookup.get(id))
        })}
      }
    })
    return {celebs : Array.from(subset),lookup}
  }
  onMount(()=>{
    celebs_promise = load_celebs()
  })

  
</script>
<main>
  {#if states ==="Welcome"}
    <Welcome onSelection={start} />
  {:else if states ==="playing"}
    <Playing {selection}/>
  {/if}
</main>

<style>
  main{
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    height: 100%;
    padding: 1em;
    margin: 0 auto;
    flex-direction: column;
    max-width: 800px;
}
</style>