<script>
  let todoList = [
    { id: 0, text: "Work on project", done: false},
    { id: 1, text: "Go to the gym", done: false},
    { id: 2, text: "If you are using the preprocess feature, then your callback responses may — in addition to the code and map values described in the Svelte compile docs", done: false}
  ]
  let todo = ""

  const addNewTodo = event => {
    event.preventDefault()
    if (todo !== "") {
      const id = todoList.length
      todoList = [...todoList, {id, text: todo, done: false}]
      todo = ""
    }
  }

  const deleteTodo = id => {
    todoList = todoList
      .filter(item => item.id !== id)
  }

  const markDoneTodo = id => {
    const oldTodo = todoList.find(item => item.id === id)
    todoList = [...todoList
      .filter(item => item.id !== id), {id, text: oldTodo.text, done: !oldTodo.done}]
      .sort((a, b) => a.id - b.id)
  }
</script>

<div class="todo-container">
  <form>
    <label class="todo-field-title" for="todo-field"><h2>Add task</h2></label>
    <input id="todo-field" class="todo-field" bind:value={todo} />
    <input type="submit" on:click={addNewTodo} value="Add" />
  </form>

  <ul class="todo-list">
    {#each todoList as item, i (item.id)}
      <li class={`item ${item.done ? 'done' : ''}`}>
        <div class="id-text-container">
          <div class="space-rs">{i + 1}.</div>
          <span>{item.text}</span>
        </div>
        <div class="controls-container">
          <button on:click={() => markDoneTodo(i)}>
            {#if (item.done)}
              <div>Undo</div>
            {:else}
              <div>Done</div>
            {/if}
          </button>
          <button aria-label="Delete task" on:click={() => deleteTodo(item.id)}>X</button>
        </div>
      </li>
    {/each}
    </ul>
</div>

<style>
  button {
    margin: 0;
  }

  ul {
    padding: 0;
  }

  li {
    list-style: none;
  }

  h2 {
    font-size: 1rem;
  }

  .todo-container, form {
    display: flex;
    flex-direction: column;
  }

  .item {
    display: flex;
    justify-content: space-between;
    border: 1px solid black;
    padding: .5rem;
    margin: 1rem 0;
    border-radius: 2px;
    line-height: 2rem;
  }

  .id-text-container {
    display: flex;
  }

  .space-rs {
    padding-right: .5rem;
  }

  .done {
    background-color: greenyellow;
  }

  .done span {
    text-decoration: line-through;
  }

  .controls-container {
    display: flex;
    align-items: start;
  }

  @media (min-width: 640px) {
		.todo-container {
			width: 50%;
		}
	}
</style>