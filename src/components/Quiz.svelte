<script lang="ts">
    import * as Dialog from '$lib/components/ui/dialog';
    import * as AlertDialog from '$lib/components/ui/alert-dialog';
    import quizQuestions from '$lib/lib/quizQuestions.json';

    let questionIndex = 0;
    let failDialogOpen = false;
    let proceedOpen = false;

    $: question = quizQuestions[questionIndex];
</script>

<Dialog.Root bind:open={failDialogOpen}>
    <Dialog.Content>
        <h1 class="text-2xl">
            Incorrect answer. Start again.
        </h1>
    </Dialog.Content>
</Dialog.Root>

<AlertDialog.Root bind:open={proceedOpen}>
    <AlertDialog.Content>
        <h1 class="text-2xl">
            Cookies quiz completed- please continue to the practical part.
        </h1>
        <AlertDialog.Footer>
            <AlertDialog.Action on:click={() => location.href = "/clicker"}>
                Continue
            </AlertDialog.Action>
        </AlertDialog.Footer>
    </AlertDialog.Content>
</AlertDialog.Root>

<div class="flex flex-col items-center">
    <h1 class="text-3xl underline mb-3">
        Please demonstrate your knowledge about cookies.
    </h1>
    <h2 class="text-xl mb-3">
        { questionIndex + 1}. {question.question}
    </h2>
    <div class="grid grid-cols-2 w-1/2">
        {#each question.answers as answer, index}
            <button class="bg-secondary p-10 text-3xl font-bold"
            style="border: 6px solid rgba(0, 0, 0, 0.3)" on:click={() => {
                if(index === question.correct) {
                    if(questionIndex === quizQuestions.length - 1) {
                        proceedOpen = true;
                    } else {
                        questionIndex++;
                    }
                } else {
                    failDialogOpen = true;
                    questionIndex = 0;
                }
            }}>
                { answer }
            </button>
        {/each}
    </div>
</div>