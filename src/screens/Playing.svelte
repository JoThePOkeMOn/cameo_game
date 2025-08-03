<script>
  import Card from "../component/Card.svelte";
  import {sleep} from '../utils.js';
  let { selection } = $props();

  const load_details = async (celebs) => {
    const res = await fetch(
      `https://cameo-explorer.netlify.app/celebs/${celebs.id}.json`
    );
    return await res.json();
  };
  const promises = selection.map((round) =>
    Promise.all([load_details(round.a), load_details(round.b)])
  );
  let i = $state(0);
  let last_result;
  const submit =async(a,b,sign)=>{
     last_result = Math.sign(a.price - b.price) === sign
      ? 'right'
      : 'wrong';
    //sleep for 1500ms
    await sleep(1500)

    if(i < selection.length -1){
        i++;
    }else{
        //todo
    }
}
</script>

<p>
  Tap on the more monetisable celebrity's face, or tap 'same price' if society
  values them equally.
</p>

<div class="game-container">
  {#await promises[i] then [a, b]}
    <div class="game">
      <div class="card-container">
        <Card 
        onselect={submit(a,b,1)}
        celeb={a} />
      </div>

      <div class="same-container">
        <button class="same" onclick={()=>{submit(a,b,0)}}>same price</button>
      </div>

      <div class="card-container">
        <Card
        onselect={submit(a,b,-1)}
         celeb={b} />
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
  .game-container {
    flex: 1;
    width: 100%; 
    height: 100%; 
  }

  .game {
    display: grid;
    /* Mobile-first layout (vertical) */
    /* grid-template-columns: 1fr; */
    grid-template-rows: 1fr 2em 1fr;
    grid-gap: 0.5em;
    width: 100%;
    height: 100%;
    max-width: min(100%,40vh);
    margin: 0 auto;
    padding: 1em;
  }

  .card-container {
    width: 100%;
    aspect-ratio: 1;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  /* Desktop layout */
  @media (min-width: 640px) {
    .game {
      grid-template-rows: none;
      grid-template-columns: 1fr 8em 1fr;
      max-height: calc(100vh - 6em);
      max-width: min(100%, 80vh);
    }
  }
</style>
