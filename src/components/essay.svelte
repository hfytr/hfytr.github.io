<script>
    let text=""
    $: words=text.split(" ").length
</script>
<h1>Usings at least 250 words, summarize the Communist Manifesto.</h1>
<textarea bind:value={text} class="bg-slate-400"></textarea>
<div>
    {text == "" ? 0 : words} wordcount:
</div>

<button on:click={()=>{
    if (words > 250){
        location.href = "/clicker"
    }
    else {
        alert("Not enough words")
    }
}}>Submit</button>