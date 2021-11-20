<script>
    import { getAuth, signInWithRedirect, GoogleAuthProvider, signOut, getRedirectResult } from "firebase/auth";
    import { userStore, tokenStore, registered } from '$lib/stores/authStore';
    import { onMount } from "svelte";
    import { getFirestore, collection, getDocs } from "firebase/firestore";

    const db = getFirestore();

    async function userExists() {
        const querySnapshot = await getDocs(collection(db, "users"));
        querySnapshot.forEach((doc) => {
            if (doc.data().id == $userStore.uid) {
                console.log('true', $userStore.uid + doc.data().id);
                registered.set(1);
            } else {
                registered.set(0);
            };
        });
        console.log($registered);
        console.log($userStore);
    }

    if (!$userStore) {
        userStore.set(null);
        tokenStore.set(null);
        registered.set(null);
    }

    let logOutVisible = false;

    function userTapped() {
        if (logOutVisible) {
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
            registered.set(null);
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

            userExists();

            console.log('token', $tokenStore);
            console.log('user', $userStore);

        }).catch((error) => {
            // Handle Errors here.
            const errorMessage = error.message;
            console.log(errorMessage);
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
                    <a href="/admin">
                        <button>Admin</button>
                    </a>
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