<script>
    let world = "world";

    function testGET() {
        console.log("ya GET me?")
    }
</script>


<div class="flex flex-col justify-center h-screen items-center">
    <h1 class="font-black text-center">Hello {world}! <br/>This is my comic book logger app</h1>
    <div class="flex gap-2 justify-center my-6">
        <button class="btn btn-primary rounded-full" onclick={testGET}> GET Data </button>
        <button class="btn btn-accent rounded-full"> Test DB Connection </button>        
    </div>

    <form class="bg-base-300 rounded-xl flex flex-col gap-4 p-6">
        <fieldset class="flex flex-col gap-2">
            <label> Add comic title </label>
            <input class="input input-text" type="text"/>
        </fieldset>
        <input class="btn btn-wide btn-warning rounded-full" value="POST data" type="submit"/>
    </form>

    <span class="mt-12 mb-8"> Data will appear here ⬇️</span>
    <table class="table">
        <thead>
            <tr>
                <td> Year </td>
                <td> Title </td>
                <td> Issue </td>
                <td> Publisher </td>
                <td> Writer </td>                
            </tr>
        </thead>
        <tbody>
            <tr>
                <td> 1900 </td>
                <td> spidey </td>
                <td> #1 </td>
                <td> MARVEL </td>
                <td> Kirby </td>                
            </tr>              
        </tbody>
      
    </table>
</div>
