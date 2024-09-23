<script>
	import { authHandlers} from "../store/store";

    let email = "";
    let password = "";
    let confirmPass = "";
    let error = false;
    let register =  false;
    let authenticating =  false;

    async function handleAuthenticate(){
        if (authenticating){
            return;
        }
        if(!email || !password || (register && !confirmPass)) {
            error = true
            return
        }
        authenticating = true;

        try {
            if (!register){
                await authHandlers.login(email, password);
            } else {
                await authHandlers.signup(email, password);
            }
        }catch (err){
            console.log('There was an auth error', err);
            error = true;
            authenticating = false;
        }
        
    }

    function handleRegister(){
        register = !register;
    }
</script>

<div class="flex flex-col items-center justify-center flex-1 p-6">

    <form class="flex flex-col gap-3 w-[400px] max-w-full m-0">
        <h1 class="poppins text-5xl text-center font-bold">{register ? "Register" : "Login"}</h1>
        {#if error}
            <p class=" text-emerald-800 text-sm text-center">The information you have entered is not correct</p>
        {/if}
        <label class="relative border border-solid border-violet-800 rounded">
            <p class="absolute -translate-y-2/4 text-white rounded px-1.5 text-sm {email ? " top-0 left-6 bg-violet-800 border border-solid border-blue-800 text-xs" : " top-2/4 left-1.5 border border-solid border-transparent opacity-0"}">Email</p>
            <input type="email" bind:value={email} class="w-full bg-transparent border-none text-white focus:border-none focus:outline-none p-[14px]" placeholder="email"/>
        </label>

        <label class="relative border border-solid border-violet-800 rounded">
            <p  class="absolute -translate-y-2/4 text-white rounded px-1.5 text-sm {password ? " top-0 left-6 bg-violet-800 border border-solid border-blue-800 text-xs" : " top-2/4 left-1.5 border border-solid border-transparent opacity-0"}">Password</p>
            <input type="password" bind:value={password} class="w-full bg-transparent border-none text-white focus:border-none focus:outline-none p-[14px]" placeholder="password"/>
        </label>

        {#if register}
        <label class="relative border border-solid border-violet-800 rounded">
            <p class="absolute -translate-y-2/4 text-white rounded px-1.5 text-sm {confirmPass ? " top-0 left-6 bg-violet-800 border border-solid border-blue-800 text-xs" : " top-2/4 left-1.5 border border-solid border-transparent opacity-0"}">Confirm Password</p>
            <input type="password" bind:value={confirmPass} class="w-full bg-transparent border-none text-white focus:border-none focus:outline-none p-[14px]" placeholder="confirm password"/>
        </label>
        {/if}

        <button 
            type="button"
            on:click={handleAuthenticate}
            class=" bg-violet-800 text-white border-none rounded cursor-pointer duration-200 hover:bg-violet-600 py-3 grid items-center"
        >
            {#if authenticating}
            <i class="fa-solid fa-spinner spinner"/>
            {:else}
            Submit
            {/if}
            
        </button>

    </form>

    <div class="px-3.5 py-0 overflow-hidden text-sm flex flex-col gap-1">
        <p class=" relative text-center w-fit mx-auto px-2 py-2">Or</p>
        {#if register}
            <div class="flex items-center gap-2 justify-center">
                <p>Already have an account?</p>
                <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
                <p class=" text-blue-300 cursor-pointer hover:text-blue-100" on:click={handleRegister} on:keydown={()=>{}}>Login</p>
            </div>
        {:else}
            <div class="flex items-center gap-2 justify-center">
                <p>Don't have an account?</p>
                <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
                <p class=" text-blue-300 cursor-pointer hover:text-blue-100" on:click={handleRegister} on:keydown={()=>{}}>Register</p>
            </div>
        {/if}
    </div>
</div>

<style>
    .spinner{
        animation: spin 2s infinite;
    }

    @keyframes spin {
        from { transform: rotate(0deg);}
        to { transform:  rotate(360deg);}
    }

</style>