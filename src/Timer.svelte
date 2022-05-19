<script>
    export let duration = 0

    $: hours = Math.floor(duration / 3600)
    $: minutes = Math.floor(duration / 60) % 60
    $: seconds = duration % 60
</script>


{#if hours > 0}
    {hours < 10 ? '0' + hours : hours}:
{/if}
{minutes < 10 ? '0' + minutes : minutes}:{seconds < 10 ? '0' + seconds : seconds}
