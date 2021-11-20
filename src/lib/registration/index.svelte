<script>
import { registered, userStore } from "$lib/stores/authStore";
import { addDoc, collection, getFirestore } from "firebase/firestore";
    
    let name = null;
    let phone = null;

    const db = getFirestore();

    async function registerUser() {
        if (phone && name) {
            if (phoneIsValid()) {
                try {
                    const docRef = await addDoc(collection(db, "users"), {
                        id: $userStore.uid,
                        name,
                        phone,
                        email: $userStore.email
                    });

                    registered.set(1);
                    console.log("Document written with ID: ", docRef.id);
                } catch (e) {
                    console.error("Error adding document: ", e);
                }
            } else {
                alert('Please Enter a valid phone number');
            }
        } else {
            alert('Please Enter all Details');
        }     
    }

    function phoneIsValid() {
        if (phone > 999999999 || phone < 100000000) {
            return false;
        }
        return true;
    }
</script>

<main>
    <h2>
        Registration
    </h2>
    <br>
    <p>Username</p>
    <input type="text" bind:value={name}>
    <br>
    <p>Phone Number</p>
    <div class="phone">
        <span>+254</span>
        <input type="number" bind:value={phone}>
    </div>
    <br>
    <button on:click={registerUser}>Register</button>
</main>

<style>
    main {
        color: black;
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .phone {
        display: flex;
    }

    span {
        margin-right: 4px;
    }
</style>