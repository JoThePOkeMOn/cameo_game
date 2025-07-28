<script>
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
                <p>{a.name}</p>
            </div>

            <div>
                <button class="same">same price</button>
            </div>

            <div class="card-container">
                <p>{b.name}</p>
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
</style>