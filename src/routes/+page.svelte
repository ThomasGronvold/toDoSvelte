<script lang="ts">
  import "../global.css";
  import { fade } from "svelte/transition";
  import { quintOut } from "svelte/easing";

  let todos = [
    { done: false, description: "write some docs" },
    { done: false, description: "start writing blog post" },
    { done: true, description: "buy some milk" },
    { done: false, description: "feed the turtle" },
    { done: false, description: "fix some bugs" },
  ];

  let newTaskInput = "";

  function addTask(description: string) {
    let task = { done: false, description: description };
    todos = [...todos, task];
    newTaskInput = "";
  }

  function deleteTask(task: { done: boolean; description: string }) {
    todos = todos.filter((i) => i !== task);
  }
</script>

<div class="mainDiv">
  <div>
    <h1>New</h1>
    <input type="text" class="newTask" bind:value={newTaskInput} />
    <button on:click={() => addTask(newTaskInput)}>Add</button>
  </div>
  <!-- to_do -->
  <div>
    <h1>Todo</h1>
    {#each todos as task}
      {#if !task.done}
        <div
          in:fade={{ delay: 300, duration: 600, easing: quintOut }}
          out:fade={{ duration: 600, easing: quintOut }}
          class="card"
        >
          <input type="checkbox" bind:checked={task.done} />
          <p>{task.description}</p>
          <button class="cardBtn" on:click={() => deleteTask(task)}>🗑️</button>
        </div>
      {/if}
    {/each}
  </div>

  <!-- done -->
  <div>
    <h1>Completed</h1>
    {#each todos as task}
      {#if task.done}
        <div
          in:fade={{ delay: 300, duration: 600, easing: quintOut }}
          out:fade={{ duration: 600, easing: quintOut }}
          class="card done"
        >
          <input type="checkbox" bind:checked={task.done} />
          <p>{task.description}</p>
          <button class="cardBtn" on:click={() => deleteTask(task)}>🗑️</button>
        </div>
      {/if}
    {/each}
  </div>
</div>

<style>
  .mainDiv {
    display: flex;
  }

  .newTask {
    font-size: 1.4em;
    padding: 0.5em;
    margin: 0 0 1rem 0;
  }

  .card {
    display: flex;
    color: white;
    border-radius: 5px;
    width: 200px;
    text-overflow: ellipsis;
    justify-content: space-between;
    background-color: rgba(0, 0, 139, 0.496);
    margin: 10px;
  }

  .done {
    opacity: 50%;
  }

  .cardBtn {
    background: none;
    border: none;
    cursor: pointer;
  }
</style>
