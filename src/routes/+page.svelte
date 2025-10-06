
<script>
import Hero from "../lib/components/hero.svelte";
import Nav from "../lib/components/nav.svelte"
import noCover from "../lib/assets/noCover.png";

let results = $state([]);

  async function testGET() {

    try {
        const data = await fetch('http://localhost:8888/api/comics.php')
        .then(res => res.json())
        .then(e => {
            console.log("fetch response is: ", e)
            e.forEach(item => {
                results.push(item)
            })
        })
    
    } catch (err) {
        console.error(err);
    } finally {
        console.log("done fetching")
    }
  }

    const badge = (rarity) => {
        let result;
        switch (rarity) {
            case 'Uncommon':
                result = 'badge-success'
                break;
            case 'Rare':
                result = 'badge-primary'
                break;
            case 'Unique':
                result = 'badge-warning'
                break;  
            default:
                result = 'badge-info'
        }
        return result
    }

</script>

<div class="flex flex-col justify-center p-4">
   <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <button onclick={testGET} class="btn btn-link link-warning mb-12"> GET all comics </button> 

   <div class="flex flex-col text-center gap-4 max-w-7xl w-full m-auto">

        <div class="grid gap-8 grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
            {#each results as comic}
                <div class="flex items-center gap-4 text-left"> 
                    <img src={comic.cover ?? noCover} class="aspect-2/3 w-32" alt={comic.title} />
                    <div class="flex flex-col gap-1">
                        <div class="font-bold text-md">{comic.title}</div>
                        <div class="badge {badge(comic.rarity)} badge-xs rounded-full">{comic.rarity}</div>
                        <div class="text-xs">Author : {comic.author}</div>
                        <div class="text-xs">Iss #{comic.issue}</div>
                        <div class="text-xs">{comic.owned > 0 ? '✅ Owned' : '❌ Unowned'} </div>
                        <div class="text-xs">{(comic.owned > 0 ? ('Paid ' + comic.price_paid) : null )}</div>
                    </div>

                </div>
            {:else}
                    <div class="flex items-center gap-4 text-left"> 
                        <img src={noCover} class="w-32" alt="nothing comic" />
                        <div class="flex flex-col gap-1">
                            <div class="badge badge-warning badge-xs rounded-full">Uniquely nothing</div>
                            <div class="font-bold text-md">Nothing comic</div>
                            <div class="text-sm">Author : Wilson</div>
                            <div class="text-xs">Iss #1</div>
                            <div class="text-xs">✅ Owned</div>
                            <div class="text-xs">Paid $1,000.00</div>
                        </div>
                    </div>
            {/each}              
        </div>
 
   </div>
</div>
