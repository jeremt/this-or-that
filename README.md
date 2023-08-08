# This Or That

This is a simple realtime game to showcase how to use Svelte and Supabase.

## Try it?

```sh
git clone git@github.com:jeremt/this-or-that.git
cd this-or-that
npm run dev
```

## Want to build it yourself?

### Setup users

Follow [this tutorial](https://supabase.com/docs/guides/getting-started/tutorials/with-sveltekit).

For _src/routes/+page.svelte_, use this code instead (easier & less dependencies) :

```svelte
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
```

You should have a working user login/profile like so :

![step_1](https://github.com/jeremt/this-or-that/assets/1913169/8acf2881-abc4-4e1f-b7e7-1bf808812c58)
