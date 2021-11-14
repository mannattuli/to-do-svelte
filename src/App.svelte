<script>
	let uid = 1;
	let todos = [];

	function add(input) {
		const todo = {
			id: uid++,
			done: false,
			description: input.value
		};

		todos = [todo, ...todos];
		input.value = '';
	}

	function remove(todo) {
		todos = todos.filter(t => t !== todo);
	}

	function mark(todo, done) {
		todo.done = done;
		remove(todo);
		todos = todos.concat(todo);
	}

	let firstName = ''

	function first(input){
		if (input.value == '' || input.value == null){
			firstName = (input.value).toUpperCase();
			input.classList.add("hidden")
		}else{
			firstName = (input.value).toUpperCase()  + "'S";;
			input.classList.add("hidden")			
		}
	}
</script>

<p class="hidden"></p>

<div class="padded">

	<div style="text-align:center">
		<input on:keydown={x => x.key === 'Enter' && first(x.target)} class="firstNamer" placeholder="add your name here"/>
	</div>

	<h1 style="text-align: center;font-weight:400; margin-bottom:5px">{firstName} TO DO LIST:</h1>
	<div class="input">
		<input
			placeholder="add a task"
			on:keydown={e => e.key === 'Enter' && add(e.target)}
		>
	</div>
	
	{#if todos.filter(t => !t.done).length == 0}
	  <h3 style="text-align: center;">You have no tasks left!</h3>
	{/if}

	<div>
		{#if todos.filter(t => !t.done).length !== 0}
			<h3>todo</h3>
		{/if}
		{#each todos.filter(t => !t.done) as todo (todo.id)}
			<label>
				<input type=checkbox on:change={() => mark(todo, true)}>
				{todo.description}
				<button on:click="{() => remove(todo)}">X</button>
			</label>
		{/each}
	</div>

	<div>
		{#if todos.filter(t => t.done).length !== 0}
			<h3>done</h3>
		{/if}
		<strike>
			{#each todos.filter(t => t.done) as todo (todo.id)}
				<label class="done">
					<input type=checkbox checked on:change={() => mark(todo, false)}>
					{todo.description}
					<button on:click="{() => remove(todo)}">X</button>
				</label>
			{/each}
		</strike>
	</div>
</div>

<style>
	.input {
		text-align: center;
	}

	input{
		width: 50%;
	}

	.hidden{
		display: none;
	}

	.padded {
		margin: auto;
		padding: 0 20%;
		max-width: 900px;
	}


	label {
		position: relative;
		line-height: 1.2;
		padding: 0.5em 2.5em 0.5em 2em;
		margin: 0 0 0.5em 0;
		border-radius: 10px;
		user-select: none;
		width: 100%;
		color: #fcfcfc;
		background-color:rgb(38, 38, 138);
	}

	input[type="checkbox"] {
		display: none;
	}

	button {
		position: absolute;
		top: 0;
		color: rgb(255, 0, 0);
		right: 0.35em;
		border: none;
		opacity: 0;
		transition: opacity 0.2s;
		cursor: pointer;
		background-color: transparent;
	}

	label:hover button {
		opacity: 1;
	}
</style>