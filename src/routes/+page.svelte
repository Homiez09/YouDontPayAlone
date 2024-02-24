<script lang="ts">
	import FormBeforeGenQR from '../components/formBeforeGenQR.svelte';
	import { page } from '$app/stores';

	let title: string = $page.url.searchParams.get('title') || '';
	let promptPayCode: string = $page.url.searchParams.get('promptPayCode') || '';
    let price = Number($page.url.searchParams.get('price')) || 0;
    let members: string[] = $page.url.searchParams.get('members')?.split(',') || [];
	let tempName: string = '';

	
</script>

<svelte:head>
	<title>YouDontPayAlone</title>
	<meta name="title" content="YouDontPayAlone" />
	<meta name="description" content="You are not alone." />
	<meta
		name="viewport"
		content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"
	/>
	<link rel="preconnect" href="https://fonts.googleapis.com">
	<link rel="preconnect" href="https://fonts.gstatic.com">
	<link href="https://fonts.googleapis.com/css2?family=Athiti:wght@500&family=Prompt:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap" rel="stylesheet">
</svelte:head>

<main class="p-10 select-none">
	<div class="h-1/2 flex flex-col items-center justify-center gap-3">
		<h1 class="md:text-5xl text-4xl">YouDontPayAlone</h1>
		<FormBeforeGenQR getURLeSearchParams={
			{
				title: title,
				promptPayCode: promptPayCode,
				price: price,
				members: members
			}
		
		}/>
	</div>
</main>
