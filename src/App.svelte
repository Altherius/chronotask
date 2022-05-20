<script lang="ts">

	import "../node_modules/bootstrap/dist/css/bootstrap.min.css";
	import Fa from 'svelte-fa/src/fa.svelte'
	import Timer from './Timer.svelte'
	import {faClock, faPause, faPlay, faPlus} from "@fortawesome/free-solid-svg-icons";

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
			<div class="row">
				<div class="col-6">
					<div class="card">
						<div class="card-body">
							<h2 class="text-center card-title">Add a task!</h2>
							<div class="row">
								<div class="col">
									<input class="form-control" bind:value={name} type="text" id="taskName" placeholder="What to do ?">
								</div>
								<div class="col">
									<input class="form-control" bind:value={duration} type="number" id="taskDuration" placeholder="5">
								</div>
							</div>

							<hr>

							<div class="text-center">
								<button class="btn btn-outline-primary" type="button" on:click={addTask}>
									<Fa icon="{faPlus}"></Fa>
									Add task
								</button>
							</div>
						</div>
					</div>
				</div>

				<div class="col-6">
					<div class="card">
						<div class="card-body">
							<h2 class="card-title text-center">Timer</h2>
							<div class="text-center">
								<button class="btn btn-outline-primary" type="button" on:click={toggleTimer} >
									{#if timerTicking}
										<Fa icon="{faPause}"></Fa>
										Pause timer
									{:else}
										<Fa icon="{faPlay}"></Fa>
										Start timer
									{/if}
								</button>
							</div>
						</div>
					</div>
				</div>
			</div>

		</div>


		<h2 class="text-center mt-4">Task list</h2>
		{#if tasks.length > 0}
			<ul class="list-group">
				{#each tasks as task, i}

					<li class="list-group-item {i === 0 ? 'list-group-item-primary' : ''}">
						<h3 class="display-5">{task.name}</h3>
						<h4>
							<Fa icon="{faClock}"></Fa>
							<Timer duration="{task.duration}" />
						</h4>
					</li>
				{/each}
			</ul>

		{:else}
			<div class="d-block alert alert-primary text-center">
				You have nothing to do, add something!
			</div>
		{/if}
	</div>
</main>

<style>
</style>