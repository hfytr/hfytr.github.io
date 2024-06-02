<script lang="ts">
    import * as Dialog from '$lib/components/ui/dialog';

    let open = true;

    $: if(!open) setTimeout(() => open = true, 50);
</script>

<Dialog.Root bind:open={open}
closeOnEscape={false} closeOnOutsideClick={false}>
    <Dialog.Content>
        <Dialog.Header>
            <Dialog.Title>
                <h1 class="font-bold text-3xl text-center">
                    Cookie Policy
                </h1>
            </Dialog.Title>
            <Dialog.Description>
                <div class = "flex items-center justify-around">
                    Please accept cookies.
                </div>
                <div class = "flex items-center justify-around mt-4">
                    <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ" class="bg-foreground border-solid border-color border-2 rounded-lg px-4 py-2 w-48 text-center">Accept</a>
                    <a href="/cookies" class="bg-foreground border-solid border-color border-2 rounded-lg px-4 py-2 w-48 text-center">Manage Preferences</a>
                </div>
            </Dialog.Description>
        </Dialog.Header>
    </Dialog.Content>
</Dialog.Root>