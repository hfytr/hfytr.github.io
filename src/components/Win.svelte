<script lang="ts">
    import JSConfetti from 'js-confetti';

    const confetti = new JSConfetti();

    let img = new Image();
    img.src = '/winScreen.png';
    img.onload = () => {
        confetti.addConfetti({
            confettiNumber: 500
        });
    }
</script>

<div class="flex flex-col items-center h-full bg-cover bg-center"
style="background-image: url('/winScreen.png')">
    <h1 class="text-5xl font-bold">You did it!</h1>
    <p class="mb-3 text-2xl">You've successfully disabled cookies. Good job!</p>
    <a href="/" class="w-40 text-center bg-muted p-1 rounded-lg">
        Start from beginning
    </a>
</div>