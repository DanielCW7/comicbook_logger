
<script>
import Hero from "../components/hero.svelte";
import Nav from "../components/nav.svelte"
import Table from "../components/table.svelte";
let results = $state([]);

  async function testGET() {

    try {
        const data = await fetch('http://localhost:8888/conn.php')
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

</script>

<Nav />
<Hero />

<div class="flex flex-col justify-center p-4">
   <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <button onclick={testGET} class="btn btn-link link-warning"> Start loggin' </button> 

   <div class="flex flex-col text-center gap-4">
        <!-- <table class="table table-xs">
            <thead>
                <tr>
                    <td> Cover </td>
                    <td> title </td>                
                    <td> Description </td>
                    <td> Author </td>
                    <td> Issue </td>
                    <td> Owned </td>                
                    <td> price_paid </td>                
                    <td> rarity </td>                
                </tr>
            </thead>
            <tbody>
              {#each results as comic}
                <tr> 
                    <td><img src={comic.cover} class="w-16" alt={comic.title} /></td>
                    <td>{comic.title}</td>
                    <td>{comic.description}</td>
                    <td>{comic.author}</td>
                    <td>{comic.issue}</td>
                    <td>{comic.owned}</td>
                    <td>{comic.price_paid}</td>
                    <td><span class="badge badge-primary bade-sm">{comic.rarity}</span></td>
                    <td>{comic.publisher}</td>
                </tr>
                {:else}
                <tr> 
                    <td>nothing here</td> 
                </tr>
              {/each}
            </tbody>
        </table>  -->
        <div class="flex gap-6">
            {#each results as comic}
                <div class="flex items-center gap-2 text-left"> 
                    <img src={comic.cover} class="w-32" alt={comic.title} />
                    <div class="flex flex-col gap-1">
                        <div class="font-bold text-md">{comic.title}</div>
                        <div class="badge badge-primary bade-xs rounded-full ">{comic.rarity}</div>
                        <div class="text-xs">Author : {comic.author}</div>
                        <div class="text-xs">Iss : {comic.issue}</div>
                        <div class="text-xs">Owned :{comic.owned > 0 ? ' ✅' : ' ❌'}</div>
                        <div class="text-xs">Paid : {comic.price_paid}</div>
                    </div>

                </div>
            {:else}
                <div> 
                    <p> No comics yet </p> 
                </div>
            {/each}              
        </div>
 
   </div>

</div>
