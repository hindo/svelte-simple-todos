<script>
  import Input from './Input.svelte';
  const ENTER_KEY = 13;
  let filter = false;
  let todos = [];

  try {
    todos = JSON.parse(localStorage.getItem('todos-svelte')) || [];
  } catch(err) {
    todos = [];
  }

  function createNew(event) {
    if (event.which === ENTER_KEY) {
      todos = [{
        id: uuid(),
        completed: false,
        description: event.target.value
        }, ...todos];
      event.target.value = '';
    }
  }

  function removeItem(index) {
    event.preventDefault();
    todos = todos.filter(todo => todo.id !== index)
  }

  function uuid() {
		return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
			var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
			return v.toString(16);
		});
  }

  $: filtered = !filter ?
    todos : todos.filter(todo => !todo.completed);

  $: try {
    localStorage.setItem('todos-svelte', JSON.stringify(todos));
  } catch(err) {
    // noop
  }
</script>

<style>
  .app {
    margin: 0 auto;
    width: 40rem;
  }

  .item {
    padding: 5px 10px;
    display: flex;
  }

  .item > * {
    height: 1.75rem;;
    line-height: 1.75rem;
    margin: 0;
    padding: 0;
    flex: 1;
  }

  .item input {
    flex-grow: 0;
    margin-right: 10px;
  }

  .item span {
    font-size: 1.2rem;
  }

  .item .icon {
    font-size: 1.4rem;
    flex-grow: 0;
  }

  .item:hover {
    background-color: rgb(245, 245, 245);
  }

  .item:hover > .icon {
    display: inline;
  }

  .icon {
    display: none;
    font-size: 1.1rem;
    font-style: normal;
    cursor: pointer;
  }
</style>

<div class="app">
  <section class="section">
      <h1 class="title">TodoMVC</h1>
      <p class="subtitle">This is some sample app to demonstrate how Svelte works and get some knowledge about it.</p>
  </section>
  <div class="section divider"></div>
  <section class="section">
    <div>
      <Input on:keydown={createNew} />
    </div>
    <div>
      {#if todos.length}
        {#each filtered as todo}
          <label class="item">
            <input type="checkbox" bind:checked={todo.completed} />
            <span>{todo.description}</span>
            <i on:click={() => removeItem(todo.id)} class="icon">&times;</i>
          </label>
        {/each}
      {:else}
        <div class="panel-block">
          <p class="title is-4">No items here!</p>
        </div>
      {/if}
    </div>
    <hr />
    <div>
      <label>
        <input type="checkbox" bind:checked={filter} />
        Hide completed
      </label>
    </div>
  </section>
</div>
