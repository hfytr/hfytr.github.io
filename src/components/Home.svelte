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
                <div>
                    Please accept cookies.
                </div>
            </Dialog.Description>
        </Dialog.Header>
    </Dialog.Content>
</Dialog.Root>