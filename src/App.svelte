<script lang="ts">

	import "../node_modules/bootstrap/dist/css/bootstrap.min.css";
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
	<div class="container-fluid">
		<h1 class="text-center">Chronotask</h1>
		<hr>

		<div>
			<h2 class="text-center">Add a task!</h2>
			<div class="row">
				<div class="col">
					<input class="form-control" bind:value={name} type="text" id="taskName" placeholder="What to do ?">
				</div>
				<div class="col">
					<input class="form-control" bind:value={duration} type="number" id="taskDuration" placeholder="5">
				</div>
			</div>

			<div class="text-center mt-3">
				<button class="btn btn-outline-primary" type="button" on:click={addTask}>Add task</button>
			</div>
		</div>

		<div>
			<h2 class="text-center">Timer</h2>
			<div class="text-center">
				<button class="btn btn-outline-primary" type="button" on:click={toggleTimer} >
					{#if timerTicking}
						Pause timer
					{:else}
						Start timer
					{/if}
				</button>
			</div>
		</div>

		<h2 class="text-center">Task list</h2>
		{#if tasks.length > 0}
			<ul class="list-group">
				{#each tasks as task}
					<li class="list-group-item">
						{task.name} <Timer duration="{task.duration}" />
					</li>
				{/each}
			</ul>

		{:else}
			<p>
				You have nothing to do, add something!
			</p>
		{/if}
	</div>
</main>

<style>
</style>