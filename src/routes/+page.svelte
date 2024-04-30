<script>
	import Entry from '../components/Entry.svelte';

	let lorebook = [
		{ id: 1, category: 'character', name: 'Marisa', description: 'Western style Eastern magician' },
		{ id: 2, category: 'location', name: 'Tar Valon', description: 'The White Tower' }
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

	$: characterCount = lorebook.filter((item) => item.category === 'character').length;
	$: locationCount = lorebook.filter((item) => item.category === 'location').length;
	$: otherCount = lorebook.filter((item) => item.category === 'other').length;
</script>

<div class="w-full bg-slate-100 rounded-md p-4 flex flex-col">
	<div class="mx-auto w-2/3 bg-slate-200 rounded-md shadow-sm flex flex-col">
		<div class="flex justify-around items-center">
			<h1 class="text-slate-950 text-4xl p-4">Lorebook</h1>
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
			<p class="p-4 text-purple-600">{characterCount} Characters</p>
			<p class="p-4 text-blue-600">{locationCount} Locations</p>
			<p class="p-4 text-orange-600">{otherCount} Other entries</p>
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
		</div>
	</div>
</div>
<div class="bg-slate-100 min-h-screen flex grid-flow-row">
	{#each displayedLorebook as entry}
		<Entry {entry} on:save={handleSave} on:delete={handleDelete} on:duplicate={handleDuplicate} />
	{/each}
</div>
