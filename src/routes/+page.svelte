<script lang="ts">
  import { onMount } from "svelte";
  import Manifest from "@mnfst/sdk";

  // Initialize the Manifest SDK.
  const manifest = new Manifest();

  // Define the Todo type.
  type Todo = { id: number; title: string; completed: boolean };
  let todos: Todo[] = [];
  let newTodo = "";

  // Fetch all todos on component mount.
  onMount(async () => {
    getTodos();
  });

  // Fetch all todos from the database.
  async function getTodos() {
    todos = (await manifest.from("todos").find()).data as Todo[];
  }

  // Add a new todo to the database.
  async function addTodo() {
    await manifest.from("todos").create({ title: newTodo, completed: false });

    // Fetch all todos again to update the list
    getTodos();
  }

  // Remove a todo from the database.
  async function removeTodo(id: number) {
    await manifest.from("todos").delete(id);
    todos = todos.filter((todo) => todo.id !== id);
  }

  // Toggle the completion status of a todo.
  async function toggleTodoCompletion(todo: Todo) {
    // Update the todo in the database.
    const updatedTodo = { ...todo, completed: !todo.completed };
    todo = await manifest.from("todos").update(todo.id, todo);

    // Reassign the array to trigger update in the UI.
    todos = todos.map((t) => (t.id === updatedTodo.id ? updatedTodo : t));
  }
</script>

<main>
  <h1>Todo App</h1>

  <!-- Input for new todo -->
  <input
    type="text"
    bind:value={newTodo}
    placeholder="Add a new task..."
    on:keydown={(e) => (e.key === "Enter" ? addTodo() : null)}
  />
  <button on:click={addTodo}>Add Todo</button>

  <!-- Display todo list -->
  <ul>
    {#each todos as todo (todo.id)}
      <li class:completed={todo.completed}>
        <input
          type="checkbox"
          checked={todo.completed}
          on:click={() => toggleTodoCompletion(todo)}
        />
        {todo.title}
        <button on:click={() => removeTodo(todo.id)}>Remove</button>
      </li>
    {/each}
  </ul>

  {#if todos.length === 0}
    <p>No todos yet. Add some!</p>
  {/if}
</main>

<style>
  main {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 0 auto;
    padding: 2rem;
  }
  h1 {
    text-align: center;
  }
  input[type="text"] {
    width: 80%;
    padding: 0.5rem;
    font-size: 1rem;
    margin-right: 0.5rem;
  }
  button {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }
  button:hover {
    background-color: #0056b3;
  }
  ul {
    list-style: none;
    padding: 0;
  }
  li {
    background-color: #f9f9f9;
    padding: 0.75rem;
    margin: 0.5rem 0;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  input[type="checkbox"] {
    margin-right: 1rem;
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
</style>
