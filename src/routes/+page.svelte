<script lang="ts">
    export let data;

    let email = "";
    let loading = false;
    let errorMessage = "";
    let successMessage = "";

    async function submit() {
        loading = true;
        errorMessage = "";
        successMessage = "";

        const {error} = await data.supabase.auth.signInWithOtp({
            email,
            options: {
                emailRedirectTo: `${data.url}/auth/callback`,
            },
        });

        if (error) {
            errorMessage = error.message;
        } else {
            successMessage = "Check your email for the magic link.";
        }

        loading = false;
    }
</script>

<form on:submit|preventDefault={submit}>
    <label for="email">Email address</label>
    <input name="email" type="email" placeholder="Your email address" bind:value={email} />
    <button>Send magic link 🪄</button>
    {#if successMessage !== ""}
        <span style="color: green">{successMessage}</span>
    {:else if errorMessage !== ""}
        <span style="color: red">{errorMessage}</span>
    {/if}
</form>
