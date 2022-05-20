<script lang="ts">

	import "../node_modules/bootstrap/dist/css/bootstrap.min.css";
	import Fa from 'svelte-fa/src/fa.svelte'
	import Timer from './Timer.svelte'
	import {faClock, faPause, faPlay, faPlus, faTimesCircle} from "@fortawesome/free-solid-svg-icons";
	import Moment from "moment";
	import {sendNotification} from "@tauri-apps/api/notification";


	let tasks = []
	let name:String = "Something new"
	let durationString:String = "00:00:00"
	let timerTicking:Boolean = false
	let ticker = null;

	function addTask() {

		let durationObject = Moment(durationString, 'HH:mm:ss');

		let duration = (durationObject.hours() * 3600) + (durationObject.minutes() * 60) + durationObject.seconds()

		tasks = [...tasks, {
			name: name ?? "Some task",
			duration: duration ?? 5
		}]

		name = "Something else"
		durationString = "00:00:00"
	}

	function removeTask(task) {
		let index = tasks.indexOf(task)
		if (index > -1) {
			tasks.splice(index, 1)
		}
		tasks = tasks
	}

	function toggleTimer() {
		timerTicking = !timerTicking

		if (timerTicking) {
			ticker = setInterval(function() {
				if (tasks.length > 0) {
					tasks[0].duration -= 1
					if (tasks[0].duration === 0) {
						sendNotification(tasks[0].name + " is due!")
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
		<h1 class="text-center mt-3">Chronotask</h1>

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
									<input class="form-control" bind:value={durationString} type="text" id="taskDuration" placeholder="5">
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

		<hr>


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
						<button on:click={() => removeTask(task)} class="btn btn-outline-danger"><Fa icon="{faTimesCircle}"></Fa> Delete task</button>
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