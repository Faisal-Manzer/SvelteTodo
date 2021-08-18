<script lang='ts'>
    import { onMount } from 'svelte';

    import TodoBody from 'components/todo.svelte';
    import type { Todo } from 'interfaces/todo.type';

    let todos: Todo[] = [];
    let loading: boolean = true;
    let currentTodo: string = '';

    const addTodo = () => {
        const now = new Date();
        todos = [...todos, { body: currentTodo, createdAt: now, updatedAt: now, }];
        currentTodo = '';

        Promise
            .resolve()
            .then(() => {
                window.localStorage.setItem('todos', JSON.stringify(todos));
            });
    }

    onMount(() => {
        loading = true;
        todos = JSON.parse(window.localStorage.getItem('todos') || '[]');
        loading = false;
    });
</script>

<h1>Todo Svelte</h1>
{#if loading}
    <h5>Loading...</h5>
{:else}
    <input bind:value={currentTodo} /> <button on:click={addTodo}>Add Todo</button>
    {#each todos as todo}
        <TodoBody todo={todo} />
    {/each}
{/if}
