<script> 
  import { onMount } from "svelte";
  import { select } from "./select.js";
  import Welcome from "./screens/Welcome.svelte"

  let state = $state("Welcome");
  let celebs_promise;
  let selection;
  
  const start = async (category)=>{
    // console.log(category)
    const {celebs,lookup} = await celebs_promise
    selection = select(celebs,lookup,category)
    state ="playing"
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
        c.similar.forEach(id =>{
          subset.add(lookup.get(id))
        })
      }
    })
    return {celebs : Array.from(subset),lookup}
  }
  onMount(()=>{
    celebs_promise = load_celebs()
  })

  
</script>
<main>
  {#if state ==="Welcome"}
    <Welcome onSelection={start} />
  {:else if state ==="playing"}
    <p>playing</p>
  {/if}
</main>

<style>
  main{
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    height: 100%;
    margin: 0 auto;
    flex-direction: column;
    max-width: 800px;
}
</style>