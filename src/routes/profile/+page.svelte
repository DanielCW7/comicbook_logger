
<script>

    import noCover from "../../lib/assets/noCover.png";

    let comicbook = $state({
        title: '',
        author: '',
        owned: 0,
        issue: 0,
        synopsis: '',
        rarity: 'Common',
        price: 0.00,
        cover: null, 
    })
    
    function upload(img) {
        const file = img.target.files[0];

        if(file) {
            const reader = new FileReader();
            reader.onload = (e) => { comicbook.cover = e.target.result }
            comicbook.cover = reader.readAsDataURL(file);
        } else {
            comicbook.cover = null; // reset state           
        }
        console.log(img, file);
    }

    const comic_upload = async (e) => {
        e.preventDefault();
        
        let data = {
            'title' : comicbook.title,
            'author' : comicbook.author,
            'owned' : comicbook.owned,
            'price' : comicbook.price,
            'issue' : comicbook.issue,
            'synopsis' : comicbook.synopsis,
            'rarity' : comicbook.rarity,
            'cover' : comicbook.cover,
        };

        let url = 'http://localhost:8888/api/comics.php';
        try {
            let send = await fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },                
                body: JSON.stringify(data),
            })
        } catch (e) {
            console.error(e)
        }
    }
</script>

<div class="p-4">
    <h2 class="text-lg text-center font-black"> Add to your collection </h2>
    <p class="text-center"> If you have a comic that you want to record, or keep track of the ones you want, add them here.</p>
    <div class="flex justify-center my-12">
        <form class="flex gap-4 p-4" onsubmit={comic_upload} method="POST" id="submit_comic" enctype="multipart/form-data">
            <div class="flex flex-col justify-center">
                <fieldset class="fieldset flex flex-col">
                    <legend class="label" for="comic_cover"> Add Cover <span class="badge badge-xs badge-info badge-outline" for="comic_title"> Optional </span></legend>
                    
                    {#if comicbook.cover} 
                        <img src={comicbook.cover} class="aspect-2/3 py-2 w-72" alt={comicbook.title} />
                    {:else}
                        <img src={noCover} class="aspect-2/3 py-2 w-full max-w-72" alt="placeholder-img"/>
                    {/if}

                    <input class="file-input file-input-ghost file-input-sm" type="file" id="comic_cover" accept=".jpg, .png" onchange={(e) => upload(e)} />
                </fieldset>
                
            </div>              
            <div class="p-2">
                <div class="flex gap-4">
                    <fieldset class="fieldset grow">
                        <label class="label" for="comic_title"> Title </label>
                        <input required class="input input-sm" type="text" id="comic_title" placeholder="Spiderman" bind:value={comicbook.title}/>
                    </fieldset>                
                    <fieldset class="fieldset grow">
                        <label class="label" for="comic_author"> Author </label>
                        <input required class="input input-sm" type="text" id="comic_author" placeholder="Kirby" bind:value={comicbook.author}/>
                    </fieldset>                    
                </div>

                <div class="flex gap-4">
                    <fieldset class="fieldset grow">
                        <label class="label" for="comic_issue"> Issue </label>
                        <input required class="input input-sm" type="number" id="comic_issue" placeholder="#1" bind:value={comicbook.issue}/>
                    </fieldset>
                    <fieldset class="fieldset grow">
                        <label class="label" for="comic_rarity"> Rarity </label>
                        <select class="select select-sm" id="comic_rarity" bind:value={comicbook.rarity}>
                            <option value="Common"> Common </option>
                            <option value="Uncommon"> Uncommon </option>
                            <option value="Rare"> Rare </option>
                            <option value="Unique"> Unique </option>
                        </select>
                    </fieldset>
                    <fieldset class="fieldset grow">
                        <label class="label" for="comic_price"> Price Paid </label>
                        <input class="input input-sm" type="number" id="comic_price" placeholder="$0.00" step="0.01" bind:value={comicbook.price}/>
                    </fieldset>                    
                </div>

                <fieldset class="fieldset grow">
                    <label class="label" for="comic_synopsis"> synopsis </label>
                    <textarea class="textarea textarea-sm w-full" type="text" id="comic_synopsis" placeholder="What's this comic about?" bind:value={comicbook.synopsis}></textarea>
                </fieldset>

                <fieldset class="fieldset flex justify-start gap-12">
                    <span class="flex flex-col justify-center gap-2">
                        <label class="label" for="comic_ownership"> Owned </label>
         
                        <input required class="radio radio-sm m-auto radio-success" type="radio" name="comic_ownership" value={1} bind:group={comicbook.owned} />
                    </span>                    
                    <span class="flex flex-col justify-center gap-2">
                        <label class="label" for="comic_ownership"> Unowned </label>
         
                        <input required class="radio radio-sm m-auto radio-error" type="radio" name="comic_ownership" value={0} bind:group={comicbook.owned} />
                    </span>
                </fieldset>                
                <button class="btn w-full btn-sm btn-warning mt-4" type="submit" id="submitter"> Save </button>                
            </div>            
        
        </form>
    </div>
    <!-- <div class="flex flex-col">
                <h2 class="text-lg text-center font-black"> Summary </h2>
        
                <div class="max-w-7xl flex gap-4 m-auto">
                    <div>
                        {#if comicbook.cover} 
                        <img src={comicbook.cover} class="aspect-2/3 py-2 w-full max-w-72" alt={comicbook.title} />
                        {:else}
                        <img src={noCover} class="aspect-2/3 py-2 w-full max-w-72" alt="placeholder-img"/>
                        {/if}  
                    </div>
                    <div class="flex flex-col gap-1 p-4">
                        <div class="badge badge-warning badge-lg rounded-full">{comicbook.rarity}</div>
                        <div class="font-bold text-xl">{comicbook.title}</div>
                        <div class="text-lg">Author : {comicbook.author}</div>
                        <div class="text-md">Iss #{comicbook.issue}</div>
                        <div class="text-md">{comicbook.owned > 0 ? '✅ Owned' : '❌ Unowned'} </div>
                        <div class="text-md">{(comicbook.owned > 0 ? ('Paid ' + comicbook.price) : null )}</div>
                        <div class="text-md break-all">
                            <div class="text-sm">Synopsis:</div>
                            <p class=" w-64 max-w-64 text-xs"> {comicbook.synopsis} </p>
                        </div>
                    </div>
                </div>


    </div> -->
</div>