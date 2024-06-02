<script lang="ts">
    import { Switch } from '$lib/components/ui/switch';
    import { Button } from '$lib/components/ui/button';
    import * as AlertDialog from '$lib/components/ui/alert-dialog';
    import cookieNames from '$lib/lib/cookies.json';

    let cookies = cookieNames.map(name => {
        return {
            name,
            enabled: false
        };
    });

    let alertOpen = false;
    let timesReenabled = 0;

    function onConfirm() {
        let enabledCookies = cookies.filter(c => c.enabled);

        if(enabledCookies.length > 0) {
            alertOpen = true;
            return;
        }

        if(timesReenabled < 3) {
            timesReenabled++;

            // enable timesReenabled cookies
            for(let i = 0; i < timesReenabled; i++) {
                let index = Math.floor(Math.random() * cookies.length);
                cookies[index].enabled = true;
            }

            alertOpen = true;
        }
    }
</script>

<AlertDialog.Root bind:open={alertOpen}>
    <AlertDialog.Content>
        <h1 class="w-full text-center text-xl">
            There are {cookies.filter(c => c.enabled).length} cookies still enabled.
            Would you like to continue?
        </h1>
        <AlertDialog.Footer>
            <AlertDialog.Cancel>
                Cancel
            </AlertDialog.Cancel>
            <AlertDialog.Action
            on:click={() => location.href = "https://www.youtube.com/watch?v=dQw4w9WgXcQ"}>
                Continue
            </AlertDialog.Action>
        </AlertDialog.Footer>
    </AlertDialog.Content>
</AlertDialog.Root>

<div class="flex flex-col items-center justify-center pt-5">
    <h1 class="text-5xl font-bold">Cookie Preferences</h1>
    <div class="grid" style="grid-template-columns: 300px 50px;">
        {#each cookies as cookie}
            <div class="text-2xl">
                {cookie.name}
            </div>
            <Switch bind:checked={cookie.enabled} />
        {/each}
    </div>
    <div class="flex items-center pb-20 pt-3">
        <Button class="bg-transparent text-white
        hover:bg-transparent hover:underline" on:click={onConfirm}>
            Confirm
        </Button>
        <AlertDialog.Root>
            <AlertDialog.Trigger asChild let:builder>
                <Button class="text-2xl" builders={[builder]}>Cancel</Button>
            </AlertDialog.Trigger>
            <AlertDialog.Content>
                <h1 class="w-full text-center text-xl">
                    Changes have been discarded.
                </h1>
                <AlertDialog.Footer>
                    <AlertDialog.Action on:click={() => location.reload()}>
                        Continue
                    </AlertDialog.Action>
                </AlertDialog.Footer>
            </AlertDialog.Content>
        </AlertDialog.Root>
    </div>
</div>