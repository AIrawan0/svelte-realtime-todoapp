<script>
    export let index;
    export let todo;
    export let editTodo;
    export let removeTodo;
</script>

<div class=" border-l border-solid border-cyan-500 py-2 px-3.5 flex items-center justify-between text-xl">
    <p class="">
        {index + 1}. {todo}
    </p>
    <div class="flex items-center gap-2 text-2xl">
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <i on:click={()=>{
            editTodo(index)
            }} on:keydown={()=>{}}
            class="fa-regular fa-pen-to-square cursor-pointer hover:text-amber-500"
        />
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <i on:click={()=>{
            removeTodo(index)
            }} on:keydown={()=>{}}
            class="fa-regular fa-trash-can cursor-pointer hover:text-amber-500"/>
    </div>
</div>

<style>

</style>