<script>
    import { getAuth, signInWithRedirect, GoogleAuthProvider, signOut, getRedirectResult } from "firebase/auth";
    import { userStore, tokenStore} from '$lib/stores/authStore';
    import { onMount } from "svelte";

    userStore.set(null);
    tokenStore.set(null);

    let logOutVisible = false;

    function userTapped() {
        if (logOutVisible == true) {
            logOutVisible = false;
        } else {
            logOutVisible = true;
        }
    }

    function login() {
        const provider = new GoogleAuthProvider();
        const auth = getAuth();
        signInWithRedirect(auth, provider);
    }

    function logout() {
        const auth = getAuth();
        signOut(auth).then(() => {
            userStore.set(null);
            tokenStore.set(null);
        }).catch((error) => {
            console.log(error);
        });
    }

    onMount(() => {
        const auth = getAuth();
        getRedirectResult(auth).then((result) => {
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
    })
</script>

<main>
    {#if $userStore}
        <div class="user-box" on:click={userTapped}>
            <div class='login-details'>
                <img src={$userStore.photoURL} alt="profile-photo">
                <p>{$userStore.displayName}</p>
            </div>
            {#if logOutVisible}
                <div class="logout">
                    <button on:click={logout}>Logout</button>
                    <button>Profile</button>
                </div>
            {/if}
        </div>
    {:else}
        <div class="user-box">
            <button on:click={login}>Login</button>
        </div>
    {/if}
</main>

<style>
    .user-box {
        height: 40px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        margin-right: 8px;
        position: relative;
    }

    .login-details {
        display: flex;
        align-items: center;
        font-size: 14px;
    }

    .logout {
        margin-top: 8px;
        position: absolute;
        transform: translateY(40px);
        border: 1px solid black;
        padding: 4px;
        border-radius: 4px;
    }

    .login-details > img {
        border-radius: 50%;
        height: 40px;
        margin-right: 8px;
    }
</style>