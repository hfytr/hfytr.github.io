<script lang="ts">
    import * as Dialog from '$lib/components/ui/dialog';

    let open = true;
    let openWrong = false;
    let src = "/images/orig.png";
    let input = "";

    $: if(!open) setTimeout(() => open = true, 50);
</script>

<Dialog.Root bind:open={open}
closeOnEscape={false} closeOnOutsideClick={false}>
    <Dialog.Content>
        <Dialog.Header>
            <Dialog.Title>
                <h1 class="font-bold text-3xl text-center text-foreground">
                    Are You A Robot?
                </h1>
            </Dialog.Title>
            <Dialog.Description>
            <img alt="Loating Captcha..." src={src}/>
            <div class = "flex items-center justify-around mt-4">
                <input class="bg-background border-solid border-color border-2 rounded-lg px-4 py-2 w-48 text-center text-foreground" bind:value={input} />
                <button class="bg-background border-solid border-color border-2 rounded-lg px-4 py-2 w-48 text-center text-foreground" on:click={() => {
                    src = "/images/altered.png";
                    if (input == "a1Z4q") {
                        location.href = "/clicker";
                    } else {
                        openWrong = true;
                    }
                }}>
                Submit
                </button>
            </div>
            </Dialog.Description>
        </Dialog.Header>
    </Dialog.Content>
</Dialog.Root>

<Dialog.Root bind:open={openWrong}
closeOnEscape={false} closeOnOutsideClick={false}>
    <Dialog.Content>
        <Dialog.Header>
            <Dialog.Title>
                <div class="min-h-36 content-center">
                    <h1 class="font-bold text-3xl text-center text-foreground">
                        Wrong Answer
                    </h1>
                </div>
            </Dialog.Title>
        </Dialog.Header>
    </Dialog.Content>
</Dialog.Root>