<script>
    import Card from "../component/Card.svelte";
    let {selection} = $props();

    const load_details = async (celebs)=>{
        const res = await fetch(`https://cameo-explorer.netlify.app/celebs/${celebs.id}.json`)
        return await res.json()
    }
    const promises = selection.map(round => Promise.all([
        load_details(round.a),
        load_details(round.b)
    ]))
    let i = $state(0);
</script>

<p>Tap on the more monetisable celebrity's face, or tap 'same price' if society values them equally.</p>

<div class="game-container">
    {#await promises[i] then [a,b] }
        <div class="game">
            <div class="card-container">
                <p><Card celeb = {a} /></p>
            </div>

            <div>
                <button class="same">same price</button>
            </div>

            <div class="card-container">
                <p><Card celeb = {b}/> </p>
            </div>
        </div>
    {:catch}
        <p class="error">Failed to load data</p>
    {/await}
</div>

<div class="result-container">
    <p>result</p>
</div>



<style>
   .game-container{
    flex: 1;
   }
   .error{
    color: red;
   }
   .game{
    display: grid;
    grid-template-rows: 1fr 2em 1fr;
    grid-gap: 0.5em;
    width: 100%;
    height: 100%;
    max-width: min(100%, 40vh);
    margin: 0 auto ;
   }

   .game > div{
    display: flex;
    align-items: center;
   }

   @media (min-width: 640px){
    .game{
    max-width: 100%;
    grid-template-rows: none;
    grid-template-columns: 1fr 8em 1fr;
   }
   .same{
    height: 8em ;
   }
}
</style>