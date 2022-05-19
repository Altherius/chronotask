<script lang="ts">

	import Timer from './Timer.svelte'

	let tasks = []
	let name:String = "Something awesome!"
	let duration:Number = 10
	let timerTicking = false
	let ticker = null;

	function addTask() {
		tasks = [...tasks, {
			name: name ?? "Some task",
			duration: duration ?? 5
		}]
	}

	function toggleTimer() {
		timerTicking = !timerTicking

		if (timerTicking) {
			ticker = setInterval(function() {
				if (tasks.length > 0) {
					tasks[0].duration -= 1
					if (tasks[0].duration === 0) {
						tasks.shift()
					}
				}

				if (tasks.length === 0) {
					clearInterval(ticker)
					timerTicking = false
				}
			}, 1000)
		} else {
			clearInterval(ticker)
		}
	}
</script>

<main>
	<h1>Chronotask</h1>

	<div>
		<input bind:value={name} type="text" id="taskName" placeholder="What to do ?">
		<input bind:value={duration} type="number" id="taskDuration" placeholder="5">
		<button type="button" on:click={addTask}>Add task</button>
	</div>

	<div>
		<button type="button" on:click={toggleTimer} >
			{#if timerTicking}
				Pause timer
			{:else}
				Start timer
			{/if}
		</button>
	</div>


	{#if tasks}
		<ul>
			{#each tasks as task}
				<li>
					{task.name} - <Timer duration="{task.duration}" />
				</li>
			{/each}
		</ul>

	{:else}
		You have nothing to do, add something!
	{/if}
</main>

<style>

</style>