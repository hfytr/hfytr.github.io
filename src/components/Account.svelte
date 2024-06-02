<script lang="ts">
    import * as Card from '$lib/components/ui/card';
    import { Input } from '$lib/components/ui/input';
    import { Label } from '$lib/components/ui/label';
    import { Button } from '$lib/components/ui/button';
    import * as DropdownMenu from '$lib/components/ui/dropdown-menu';
    import { Switch } from '$lib/components/ui/switch';
    import questions from '$lib/lib/questions.json';
    import Captcha from '$lib/components/Captcha.svelte';

    let page = 0;
    let canAdvance = false;

    let name = "";
    let email = "";
    let phoneNumber = "";
    let password = "";
    let hasBeenClicked = false;

    function validateEmail(email: string) {
        var emailRegEx = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;  
        return emailRegEx.test(String(email).toLowerCase());
    }

    function validatePhoneNumber(number: string) {
        var regex = /^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/im;
        return regex.test(number);
    }

    function handleSubmission() {
        hasBeenClicked = true;

        return isValidName && isValidEmail && isValidPhone && isValidPassword;
    }

    $: isValidName = name.length > 0;
    $: isValidEmail = validateEmail(email);
    $: isValidPhone = validatePhoneNumber(phoneNumber);
    $: isValidPassword = password.length > 8;

    let pass1 = '';
    let pass2 = '';
    let pass3 = '';
    let petName = '';
    let bookChosen = '';
    let cookies1 = true;
    let cookies2 = true;

    let captchaOpen = false;

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
                <section>
                    <div class="input-wrapper">
                        <Label>Full Name</Label>
                        <Input type="text" bind:value={name} />
                        {#if hasBeenClicked && !isValidName} 
                            <p class="validation-error">Please enter a name</p>
                        {/if}
                    </div>
                    <div class="input-wrapper">
                        <Label>Email</Label>
                        <Input type="text" bind:value={email} />
                        {#if hasBeenClicked && !isValidEmail} 
                            <p class="validation-error text-xs mb-2">Invalid email</p>
                        {/if}
                    </div>
                </section>
                
                <section>
                    <div class="input-wrapper">
                    <Label>Phone Number</Label>
                    <Input type="text" bind:value={phoneNumber} />
                    {#if hasBeenClicked && !isValidPhone} 
                        <p class="validation-error text-xs">Invalid phone number</p>
                    {/if}
                    </div>
                </section>
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
                        <Switch checked={Math.random() > 0.3} />
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
                        <Switch checked={Math.random() > 0.3} />
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
                        <Switch checked={Math.random() > 0.3} />
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
                        else captchaOpen = true;
                    } else if (page !== 0) {
                        page++
                    }
                    if (page === 0 && handleSubmission()) {
                        page++
                    }
                }}>
                    Next
                </Button>
            </div>
        </Card.Footer>
    </Card.Root>
</div>