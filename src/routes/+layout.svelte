<script>
  import "../app.css";
  import { onMount } from "svelte";
  import { auth, db } from "../lib/firebase/firebase";
  import { getDoc, doc, setDoc } from "firebase/firestore";
  import { authStore } from "../store/store";

  const nonAuthRoutes = ['/', 'product']

  onMount(()=>{
    console.log('Mounting');
    const unsubscribe = auth.onAuthStateChanged(async user => {
      const currentPath = window.location.pathname;

      if (!user && !nonAuthRoutes.includes(currentPath)){
        window.location.href = "/";
        return;
      }

      if (user && currentPath =='/'){
        window.location.href = "/dashboard";
        return;
      }

      if (!user) {
        return;
      }

      let dataToSetToStore;
      const docRef = doc(db, 'user', user.uid);
      const docSnap = await getDoc(docRef);
      if (!docSnap.exists()) {
        const userRef = doc(db, 'user', user.uid);
        dataToSetToStore = {
          email : user.email,
          todos :[],
        }
        await setDoc(
          userRef,
          dataToSetToStore,
          {merge : true}
        );
      }else {
        const data = docSnap.data();
        const userData =docSnap.data();
        dataToSetToStore = userData;
      }
      authStore.update(curr => {
        return  {
          ...curr,
          user, 
          data :dataToSetToStore,
          loading: false,
        }
      })

    })
  })
</script>
  
<div class="  relative flex flex-col mx-auto w-full min-h-screen text-white bg-slate-800">

  <slot />
</div>