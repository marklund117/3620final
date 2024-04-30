<!-- Entry.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';
    export let entry;
    export let lorebook;

    let editing = false;
    let dispatch = createEventDispatcher();

    function editEntry() {
        editing = true;
    }

    function saveEntry() {
        editing = false;
        dispatch('save', entry);
    }

    function deleteEntry() {
        lorebook = lorebook.filter((item) => item.id !== entry.id);
        dispatch('delete', entry);
    }

    function duplicateEntry() {
        let newEntry = { ...entry, id: Date.now(), name: `${entry.name} 2` };
        lorebook = [...lorebook, newEntry];
        dispatch('duplicate', newEntry);
    }
</script>

<div class="flex flex-col items-center justify-between rounded-md shadow-sm p-4 mt-4 w-1/5 h-1/5 mx-auto" class:bg-purple-200={entry.category === 'character'} class:bg-blue-200={entry.category === 'location'} class:bg-orange-200={entry.category === 'other'}>
    {#if editing}
        <input type="text" class="p-2 m-2 rounded-md border border-slate-300" bind:value={entry.name} />
        <select bind:value={entry.category} class="p-2 m-2 rounded-md border border-slate-300">
            <option value="character">Character</option>
            <option value="location">Location</option>
            <option value="other">Other</option>
        </select>
        <textarea class="p-2 m-2 rounded-md border border-slate-300 w-full h-20" bind:value={entry.description}></textarea>
        <button class="bg-teal-200 p-2 m-2 rounded-md border border-slate-500 hover:bg-teal-300" on:click={saveEntry}>Save</button>
    {:else}
        <h2 class="text-slate-950 text-xl">{entry.name}</h2>
        <p class="text-slate-700">{entry.description}</p>
        <button class="bg-teal-200 p-2 m-2 rounded-md border border-slate-500 hover:bg-teal-300" on:click={editEntry}>Edit</button>
    {/if}
    <button class="bg-teal-200 p-2 m-2 rounded-md border border-slate-500 hover:bg-teal-300" on:click={duplicateEntry}>Duplicate</button>
    <button class="bg-red-200 p-2 m-2 rounded-md border border-slate-500 hover:bg-red-300" on:click={deleteEntry}>Delete</button>
</div>


