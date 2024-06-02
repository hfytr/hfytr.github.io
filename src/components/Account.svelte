<script lang="ts">
    import * as Card from '$lib/components/ui/card';
    import { Input } from '$lib/components/ui/input';
    import { Label } from '$lib/components/ui/label';
    import { Button } from '$lib/components/ui/button';
    import * as DropdownMenu from '$lib/components/ui/dropdown-menu';
    import { Switch } from '$lib/components/ui/switch';
    import questions from '$lib/lib/questions.json';

    let page = 0;
    let canAdvance = false;

    let name = '';
    let email = '';
    let pass1 = '';
    let pass2 = '';
    let pass3 = '';
    let petName = '';
    let bookChosen = '';
    let cookies1 = true;
    let cookies2 = true;

    $: switch(page) {
        case 0:
            canAdvance = name.length > 0 && email.length > 0;
            break;
        case 1:
            canAdvance = pass1.length > 0 && pass1 === pass2 && pass1 === pass3;
            break;
        case 2:
            canAdvance = petName.length > 0;
            break;
        case 3:
            canAdvance = bookChosen != '';
            break;
        case 4:
            canAdvance = true;
            break;
    }
</script>

<div class="flex flex-col items-center pt-20">
    <Card.Root class="w-1/3">
        <Card.Content>
            <h1 class="font-bold w-full text-center text-2xl pt-3">
                {#if page <= 1}
                    Account Creation
                {:else}
                    Security Questions
                {/if}
            </h1>
            {#if page === 0}
                <Label for="name" class="text-lg">
                    Name
                </Label>
                <Input id="name" placeholder="Your Name" bind:value={name} />
                <Label for="email" class="text-lg pt-3">
                    Email
                </Label>
                <Input id="email" type="email" placeholder="me@gmail.com" bind:value={email} />
            {:else if page === 1}
                <Label for="password" class="text-lg pt-3">
                    Password
                </Label>
                <Input id="password" type="password" placeholder="MyPass01" bind:value={pass1} />
                <Label for="password2" class="text-lg pt-5">
                    Confirm Password
                </Label>
                <Input id="password2" type="password" placeholder="MyPass01" bind:value={pass2} />
                <Label for="password3" class="text-lg pt-5">
                    Extra Confirm Password
                </Label>
                <Input id="password3" type="password" placeholder="MyPass01" bind:value={pass3} />
            {:else if page === 2}
                <Label for="petName" class="text-lg pt-5">
                    Enter the name of your first pet
                </Label>
                <Input id="petName" type="password" placeholder="MyPass01" bind:value={petName} />
            {:else if page === 3}
                <div class="flex justify-center pt-4 gap-5">
                    Have you read 1984?
                    <DropdownMenu.Root>
                        <DropdownMenu.Trigger>
                            <Button>
                                {bookChosen ? bookChosen : 'Answer'}
                            </Button>
                        </DropdownMenu.Trigger>
                        <DropdownMenu.Content>
                            <DropdownMenu.Item on:click={() => bookChosen = 'Yes'}>
                                Yes
                            </DropdownMenu.Item>
                            <DropdownMenu.Sub>
                                <DropdownMenu.SubTrigger>
                                    No
                                </DropdownMenu.SubTrigger>
                                <DropdownMenu.SubContent>
                                    <DropdownMenu.Item on:click={() => bookChosen = 'No'}>
                                        I plan to
                                    </DropdownMenu.Item>
                                    <DropdownMenu.Item on:click={() => bookChosen = 'No'}>
                                        I might
                                    </DropdownMenu.Item>
                                    <DropdownMenu.Item on:click={() => bookChosen = 'No'}>
                                        I will never
                                    </DropdownMenu.Item>
                                </DropdownMenu.SubContent>
                            </DropdownMenu.Sub>
                        </DropdownMenu.Content>
                    </DropdownMenu.Root>
                </div>
            {:else if page === 4}
                {#each questions.slice(0, 23) as question}
                    <div class="flex items-center pt-5 gap-5 w-full justify-between">
                        <div class="text-lg">
                            {question}
                        </div>
                        <Switch checked />
                    </div>
                {/each}
                <div class="flex items-center pt-5 gap-5 w-full justify-between">
                    <div class="text-lg">
                        Do you agree to all cookies?
                    </div>
                    <Switch bind:checked={cookies1} />
                </div>
                {#each questions.slice(23, 31) as question}
                    <div class="flex items-center pt-5 gap-5 w-full justify-between">
                        <div class="text-lg">
                            {question}
                        </div>
                        <Switch checked />
                    </div>
                {/each}
                <div class="flex items-center pt-5 gap-5 w-full justify-between">
                    <div class="text-lg">
                        Do you agree to all cookies?
                    </div>
                    <Switch bind:checked={cookies2} />
                </div>
                {#each questions.slice(31) as question}
                    <div class="flex items-center pt-5 gap-5 w-full justify-between">
                        <div class="text-lg">
                            {question}
                        </div>
                        <Switch checked />
                    </div>
                {/each}
            {/if}
        </Card.Content>
        <Card.Footer>
            <div class="flex justify-end w-full">
                <Button disabled={!canAdvance}
                on:click={() => {
                    if(page === 4) {
                        if(cookies1 || cookies2) location.href = '/lose';
                        // else trigger captcha dialog
                    } else {
                        page++
                    }
                }}>
                    Next
                </Button>
            </div>
        </Card.Footer>
    </Card.Root>
</div>