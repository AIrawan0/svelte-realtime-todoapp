<script>
    import Todoitem from "../../components/Todoitem.svelte";
    import { db } from "../../lib/firebase/firebase";
    import { authHandlers, authStore } from "../../store/store";
    import { getDoc, doc, setDoc } from "firebase/firestore";

    let todoList = [];
    let currTodo = '';
    let error = false;
    
    authStore.subscribe(curr => {
        todoList = curr.data.todos;
    })

    function addTodo(){
        if(!currTodo){
            error = true;
        }
        todoList = [...todoList, currTodo];
        currTodo = "";
    }

    function editTodo(index){
        let newTodoList = todoList.filter((val, i) => {
            return i !== index;
        });

        currTodo = todoList[index];
        todoList = newTodoList;
    }

    function removeTodo(index){
        let newTodoList = todoList.filter((val, i) => {
            return i !== index;
        });

        todoList = newTodoList;
    }

    async function saveTodos(){
        try {
            const userRef = doc(db, "user", $authStore.user.uid);
            await setDoc(
                userRef,
                {
                    todos: todoList,
                },
                { merge : true}
            );
        }catch (err){
            console.log('There was an error saving your information');
        }
    }

</script>

{#if !$authStore.loading}
    <!-- Main container -->
    <div class="flex flex-col  min-h-screen gap-6 p-6 w-full max-w-[1000px] mx-auto">
        <div class="flex items-center justify-between">
            <h1 class="text-5xl font-bold">Todo List</h1>
            <div class="flex items-center gap-3.5">
                <button
                    on:click={saveTodos}
                    class=" bg-slate-700 text-white py-2.5 px-4 border-none rounded font-semibold items-center gap-3 flex hover:opacity-70 cursor-pointer"
                >
                    <i class="fa-regular fa-floppy-disk text-xl"></i><p>Save</p></button>
                    
                <button
                    on:click={authHandlers.logout}
                    class=" bg-slate-700 text-white py-2.5 px-4 border-none rounded font-semibold items-center gap-3 flex hover:opacity-70 cursor-pointer"
                >
                    <i class="fa-solid fa-right-from-bracket text-xl"></i><p>Logout</p></button>
            </div>
            
        </div>

        <!-- ITEM TODOS -->
        <main class="flex flex-col gap-2 flex-1">
            {#if todoList.length === 0}
                <p class="">
                    You have nothing todo!
                </p>
            {/if}
            {#each todoList as todo, index}
                <Todoitem todo={todo} index={index} removeTodo={removeTodo} editTodo={editTodo}/>
            {/each}
        </main>

        <!-- Enter Todo -->
        <div class={"flex items-stretch border border-solid border-cyan-800 rounded overflow-hidden" + (error ? " border-amber-500" : "")}>
            <input type="text" bind:value={currTodo} placeholder="Enter todo" class=" bg-transparent border-none p-3.5 text-white flex-1 focus:outline-none"/>
            <button on:click={addTodo} class="px-5 bg-slate-700 border-none text-cyan-200 font-bold cursor-pointer hover:bg-transparent">ADD</button>
        </div>
    </div>
{/if}
<style>

</style>