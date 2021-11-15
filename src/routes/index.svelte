<script>
    import { getAuth, signInWithPopup, GoogleAuthProvider } from "firebase/auth";
    import { userStore, tokenStore} from '$lib/stores/authStore';

    userStore.set(null);
    tokenStore.set(null);

    function login() {
        const provider = new GoogleAuthProvider();
        const auth = getAuth();
        signInWithPopup(auth, provider)
        .then((result) => {
            // This gives you a Google Access Token. You can use it to access the Google API.
            const credential = GoogleAuthProvider.credentialFromResult(result);
            tokenStore.set(credential.accessToken);
            // The signed-in user info.
            userStore.set(result.user);
            // ...

            console.log('token', $tokenStore);
            console.log('user', $userStore);

        }).catch((error) => {
            // Handle Errors here.
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorMessage);
            // The email of the user's account used.
            const email = error.email;
            // The AuthCredential type that was used.
            const credential = GoogleAuthProvider.credentialFromError(error);
            // ...
        });
    }
</script>

<main>
    {#if $userStore}
        <div class="user-box">
            <img src={$userStore.photoURL} alt="profile-photo">
            <p>{$userStore.displayName}</p>
        </div>
    {:else}
        <button on:click={login}>Login</button>
    {/if}
</main>

<style>
    
</style>