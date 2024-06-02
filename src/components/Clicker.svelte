<script lang="ts">
    import { onDestroy } from "svelte";

    let iframe: HTMLIFrameElement;

    let cookiesGotten = 0;
    let cookiesPopupAppeared = false;
    let interval = setInterval(() => {
        cookiesGotten = (iframe?.contentWindow as any)?.Game?.cookies ?? 0;
        if(iframe.contentDocument?.querySelector(".cc_btn_accept_all")) {
            if(!cookiesPopupAppeared) cookiesPopupAppeared = true;
        } else {
            if(cookiesPopupAppeared) {
                // the user dismissed the cookies popup
                location.href = "/lose"
            }
        }
    }, 50)

    onDestroy(() => {
        clearInterval(interval);
    })
</script>

<div class="w-full flex flex-col items-center">
    <h1 class="w-1/2 text-2xl font-bold text-center mb-3">
        To prove you are worthy of changing your cookies,
        please obtain 10,000 cookies in Cookie Clicker.
    </h1>
    <progress class="w-1/2 mb-3" value={cookiesGotten} max="10000"></progress>
    <iframe src="/cookieClickerEmbed" title="Cookie Clicker"
    class="w-2/3 aspect-video" bind:this={iframe}></iframe>
</div>