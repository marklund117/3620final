<script>
	import Entry from '../components/Entry.svelte';

	let title = 'Lorebook';

	let lorebook = [
		{ id: 1, category: 'character', name: 'Marisa', description: 'Western style Eastern magician' },
		{ id: 2, category: 'location', name: 'Tar Valon', description: 'The White Tower' },
		{ id: 3, category: 'other', name: 'Arbalist V', description: 'HMG manufactured by Techman' }
	];

	let newItemName = '';
	let newItemCategory = '';
	let newItemDesc = '';

	function addEntry() {
		if (newItemName && newItemCategory) {
			lorebook = [
				...lorebook,
				{
					id: Date.now(),
					category: newItemCategory,
					name: newItemName,
					description: newItemDesc
				}
			];
			newItemName = '';
			newItemCategory = '';
			newItemDesc = '';
		}
		displayedLorebook = lorebook
	}

	let categories = ['character', 'location', 'other'];

	function handleSave(event) {
		const index = lorebook.findIndex((item) => item.id === event.detail.id);
		lorebook[index] = event.detail;
	}

	function handleDelete(event) {
		lorebook = lorebook.filter((item) => item.id !== event.detail.id);
		displayedLorebook = displayedLorebook.filter((item) => item.id !== event.detail.id);
	}

	function handleDuplicate(event) {
		lorebook = [...lorebook, event.detail];
		displayedLorebook = [...displayedLorebook, event.detail];
	}

	let displayedLorebook = [...lorebook];

	function filterToCategory(category) {
		displayedLorebook = lorebook.filter((item) => item.category === category);
		console.log(`FilterToCategory has filtered to: ${category}`)
	}

	function deleteAll() {
		lorebook = [];
		displayedLorebook = [];
	}

	$: characterCount = lorebook.filter((item) => item.category === 'character').length;
	$: locationCount = lorebook.filter((item) => item.category === 'location').length;
	$: otherCount = lorebook.filter((item) => item.category === 'other').length;

	let isEditingTitle = false

</script>
<main class="bg-slate-100 h-screen">

<div class="w-full bg-slate-100 rounded-md p-4 flex flex-col">
	<div class="mx-auto w-3/4 bg-slate-200 rounded-md shadow-sm flex flex-col">
		<div class="flex justify-around items-center">
			{#if isEditingTitle}
				<input
					type="text"
					class="p-2 m-2 rounded-md border border-slate-300"
					placeholder="Lorebook Title"
					bind:value={title}
				/>
				<button
					class="bg-teal-200 p-2 m-2 rounded-md border border-slate-500 hover:bg-teal-300"
					on:click={() => (isEditingTitle = !isEditingTitle)}
				>
					Save Title
				</button>
			{:else}
				<h1 class="text-slate-950 text-4xl p-4">{title}</h1>
				<button
					class="bg-slate-300 p-2 m-2 rounded-md border border-slate-500 hover:bg-slate-400"
					on:click={() => (isEditingTitle = !isEditingTitle)}
				>
					Edit Title
				</button>
			{/if}
			<form on:submit|preventDefault={addEntry}>
				<div class="flex flex-row">
					<input
						type="text"
						class="p-2 m-2 rounded-md border border-slate-300"
						placeholder="Entry name"
						bind:value={newItemName}
					/>
					<select bind:value={newItemCategory} class="p-2 m-2 rounded-md border border-slate-300">
						<option value="">Select a category</option>
						<option value="character">Character</option>
						<option value="location">Location</option>
						<option value="other">Other</option>
					</select>
					<button
						type="submit"
						class="bg-teal-200 p-2 m-2 rounded-md border border-slate-500 hover:bg-teal-300"
					>
						Add Entry
					</button>
				</div>
				<input
					type="text"
					class="p-2 m-2 rounded-md border border-slate-300 w-full h-20"
					placeholder="Entry description"
					bind:value={newItemDesc}
				/>
			</form>
		</div>
		<div class="flex justify-between items-center bg-slate-200">
			<p class="p-4 text-purple-600">{characterCount} Character(s)</p>
			<p class="p-4 text-blue-600">{locationCount} Location(s)</p>
			<p class="p-4 text-orange-600">{otherCount} Other(s)</p>
			<button
				class="bg-purple-100 p-2 m-2 rounded-md border border-slate-500 hover:bg-purple-200"
				on:click={() => {
					console.log(`Character Filter Button Clicked.`)
					filterToCategory('character')
					}}
			>
				Show Characters
			</button>
			<button
				class="bg-blue-100 p-2 m-2 rounded-md border border-slate-500 hover:bg-blue-200"
				on:click={() => {
					console.log(`Location Filter Button Clicked.`)
					filterToCategory('location')
					}}
			>
				Show Locations
			</button>
			<button
				class="bg-orange-100 p-2 m-2 rounded-md border border-slate-500 hover:bg-orange-200"
				on:click={() => {
				console.log(`Other Filter Button Clicked.`)
				filterToCategory('other')
				}}
			>
				Show Others
			</button>
			<button
				class="bg-slate-300 p-2 m-2 rounded-md border border-slate-500 hover:bg-slate-400"
				on:click={() => (displayedLorebook = lorebook)}
			>
				Show All
			</button>
			<button
				class="bg-slate-300 p-2 m-2 rounded-md border border-slate-500 hover:bg-slate-400"
				on:click={() => (displayedLorebook = displayedLorebook.sort((a, b) => a.name.localeCompare(b.name)))}
			>
				Sort A-Z
			</button>
			<button
				class="bg-red-200 p-2 m-2 rounded-md border border-slate-500 hover:bg-red-300"
				on:click={deleteAll}
			>
				Delete All
			</button>
		</div>
	</div>
</div>

<div class="bg-slate-100 flex flex-wrap mx-auto w-3/4">
	{#each displayedLorebook as entry}
		<Entry {entry} on:save={handleSave} on:delete={handleDelete} on:duplicate={handleDuplicate} />
	{/each}
</div>

</main>
