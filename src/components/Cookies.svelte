<script lang="ts">
    import { Switch } from '$lib/components/ui/switch';
    import { Button } from '$lib/components/ui/button';
    import * as AlertDialog from '$lib/components/ui/alert-dialog';
    import cookieNames from '$lib/lib/cookies.json';

    let cookies = cookieNames.map(name => {
        return {
            name,
            enabled: true
        };
    });

    let cookiesEnabledOpen = false;
    let timesReenabled = 0;

    let confirmOpen = false;
    let activeConfirmation = 0;

    function onConfirm() {
        let enabledCookies = cookies.filter(c => c.enabled);

        if(enabledCookies.length > 0) {
            cookiesEnabledOpen = true;
            return;
        }

        if(timesReenabled < 3) {
            timesReenabled++;

            // enable timesReenabled cookies
            for(let i = 0; i < timesReenabled; i++) {
                let index = Math.floor(Math.random() * cookies.length);
                cookies[index].enabled = true;
            }

            cookiesEnabledOpen = true;
        } else {
            confirmOpen = true;
        }
    }

    function closeConfirmations() {
        confirmOpen = false;
        activeConfirmation = 0;
    }

    let makeAccountOpen = false;

    function nextConfirmation(e: any) {
        e.preventDefault();
        activeConfirmation++;

        if(activeConfirmation >= cookies.length - 1) {
            confirmOpen = false;
            activeConfirmation = 0;
            makeAccountOpen = true;
        }
    }
</script>

<AlertDialog.Root bind:open={makeAccountOpen}>
    <AlertDialog.Content>
        <h1 class="text-xl text-center">
            Before making any changes you need to make an account.
        </h1>

        <AlertDialog.Footer>
            <AlertDialog.Action on:click={() => {
                location.href = "/account"
            }}>
                Continue
            </AlertDialog.Action>
        </AlertDialog.Footer>
    </AlertDialog.Content>
</AlertDialog.Root>

<AlertDialog.Root bind:open={confirmOpen}>
    <AlertDialog.Content>
        <h1>
            You have disabled {cookies[activeConfirmation].name}!
            Are you sure you want to continue?
        </h1>
        <AlertDialog.Footer style={activeConfirmation >= 16 ? 'justify-content: start' : ''}>
            {#if activeConfirmation < 9}
                <AlertDialog.Cancel on:click={closeConfirmations}>
                    Cancel
                </AlertDialog.Cancel>
            {/if}
            {#if activeConfirmation < 16}
                <AlertDialog.Action on:click={nextConfirmation}>
                    Continue
                </AlertDialog.Action>
            {:else}
                <AlertDialog.Cancel on:click={nextConfirmation}>
                    Continue
                </AlertDialog.Cancel>
            {/if}
            {#if activeConfirmation >= 9}
                {#if activeConfirmation < 16}
                    <AlertDialog.Cancel on:click={closeConfirmations}>
                        Cancel
                    </AlertDialog.Cancel>
                {:else}
                    <AlertDialog.Action on:click={closeConfirmations}>
                        Cancel
                    </AlertDialog.Action>
                {/if}
            {/if}
        </AlertDialog.Footer>
    </AlertDialog.Content>
</AlertDialog.Root>

<AlertDialog.Root bind:open={cookiesEnabledOpen}>
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
            on:click={() => location.href = "/lose"}>
                Continue
            </AlertDialog.Action>
        </AlertDialog.Footer>
    </AlertDialog.Content>
</AlertDialog.Root>

<div class="flex flex-col items-center justify-center pt-5">
    <h1 class="text-5xl font-bold">Cookie Preferences</h1>
    <div class="grid" style="grid-template-columns: 450px 50px;">
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