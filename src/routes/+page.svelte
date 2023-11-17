<script lang="ts">
	type Todo = {
		text: string;
		done: boolean;
	};
	type Filters = 'all' | 'active' | 'completed';

	let todos = $state<Todo[]>([]);
	let filter = $state<Filters>('all');
	let filteredTodos = $derived(filterTodos());

	$effect(() => {
		const savedTodos = localStorage.getItem('todos');
		savedTodos && (todos = JSON.parse(savedTodos));
	});

	$effect(() => {
		localStorage.setItem('todos', JSON.stringify(todos));
	});

	function addTodo(event: KeyboardEvent) {
		if (event.key !== 'Enter') return;

		const todoEl = event.target as HTMLInputElement;
		const text = todoEl.value;
		const done = false;

		if (!text) return;

		todos = [...todos, { text, done }];

		todoEl.value = '';
	}

	function editTodo(event: Event) {
		const inputEl = event.target as HTMLInputElement;
		const index = +inputEl.dataset.index!;
		todos[index].text = inputEl.value;
	}

	function toggleTodo(event: Event) {
		const inputEl = event.target as HTMLInputElement;
		const index = +inputEl.dataset.index!;
		todos[index].done = !todos[index].done;
	}

	function setFilter(newFilter: Filters) {
		filter = newFilter;
	}

	function clearCompleted() {
		todos = todos.filter((todo) => !todo.done);
	}

	function clearAll() {
		todos = [];
	}

	function filterTodos() {
		switch (filter) {
			case 'all':
				return todos;
			case 'active':
				return todos.filter((todo) => !todo.done);
			case 'completed':
				return todos.filter((todo) => todo.done);
		}
	}

	function remaining() {
		return todos.filter((todo) => !todo.done).length;
	}

	function capitalize(str: string) {
		return str.charAt(0).toUpperCase() + str.slice(1);
	}
</script>

<svelte:head>
	<title>Svelte 5 Todo</title>
</svelte:head>

<input onkeydown={addTodo} placeholder="Add todo" type="text" />

<div class="todos">
	{#each filteredTodos as todo, i}
		<div class:completed={todo.done} class="todo">
			<input oninput={editTodo} data-index={i} value={todo.text} disabled={todo.done} type="text" />
			<input onchange={toggleTodo} data-index={i} checked={todo.done} type="checkbox" />
		</div>
	{/each}
</div>

<div class="filters">
	{#each ['all', 'active', 'completed'] as filter}
		<button onclick={() => setFilter(filter)}>{capitalize(filter)}</button>
	{/each}
</div>

<div class="clears">
	<button class="clear" disabled={remaining() === todos.length} onclick={clearCompleted}>
		Clear completed
	</button>
	<button class="clear" disabled={remaining() === 0} onclick={clearAll}>Clear all</button>
</div>

<p>{remaining()} remaining</p>

<style>
	.todos {
		display: grid;
		gap: 1rem;
		margin-block-start: 1rem;
	}

	.todo {
		position: relative;
		transition: opacity 0.3s;
	}

	.completed {
		opacity: 0.3;
	}

	input[type='text'] {
		width: 100%;
		padding: 1rem;
	}

	input[type='checkbox'] {
		position: absolute;
		right: 4%;
		top: 50%;
		translate: 0% -50%;
	}

	.clears {
		display: flex;
		gap: 1rem;
	}

	.clear {
		width: 100%;
		white-space: nowrap;
	}

	.filters {
		display: flex;
		gap: 1rem;
		margin-block: 1rem;
	}
</style>
