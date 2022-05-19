<script>
    export let duration = 0

    $: minutes = Math.floor(duration / 60)
    $: seconds = duration % 60
</script>

<main>
    {minutes < 10 ? '0' + minutes : minutes}:{seconds < 10 ? '0' + seconds : seconds}
</main>