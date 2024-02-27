<script lang="ts">
	import GenQR from '../../components/genQR.svelte';

	import { page } from '$app/stores';
	import { error } from '@sveltejs/kit';
	import toast, { Toaster } from 'svelte-french-toast';
	
	let title: string = $page.url.searchParams.get('title') || '';
	let promptPayCode: string = $page.url.searchParams.get('promptPayCode') || '';
    let price = Number($page.url.searchParams.get('price')) || 0;
    let members: string[] = $page.url.searchParams.get('members')?.split(',') || ['Me'];

	let check: number = 0;

	['title', 'promptPayCode', 'price', 'members'].forEach((ele) => {
		if ($page.url.searchParams.get(ele)) {
			check++;
		}
	});

	const shareButton = () => {
        navigator.share({
            title: title,
            text: `Total Price: ${price}`,
            url: window.location.href
        });
    }

    const editButton = () => {
        window.location.href = `/?title=${title}&promptPayCode=${promptPayCode}&price=${price}&members=${members.join(',')}`;
    };

</script>

<main class="p-10 select-none">
	<div class="h-1/2 flex flex-col items-center justify-center gap-3">
		<h1 class="md:text-5xl text-4xl">YouDontPayAlone</h1>
		{#if check < 4}
			<p class="text-red-500">Sorry, something went wrong</p>
		{:else}
			<GenQR getURLeSearchParams={
				{
					title: title,
					promptPayCode: promptPayCode,
					price: price,
					members: members
				}
			}/>
			<div class="flex justify-center w-1/2 gap-3">
				<button class="btn btn-info w-full" on:click={() => shareButton()}>Share</button>
				<button class="btn btn-warning w-full" on:click={() => editButton()}>Edit</button>
			</div>
		{/if}
		
	</div>
</main>