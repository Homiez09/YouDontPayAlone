<script lang="ts">
	import Members from '../components/members.svelte';
	import promptQR from 'promptpay-qr';
	import QRCode from 'qrcode';

	let title: string = '';
	let price: number;
	let amountResult: number;
	let members: string[] = ['Me'];
	let tempName: string = '';
	let promptPayCode: string = '';
	let error: boolean = false;
	let chkGenerate: boolean = false;
	let messageError: string;

	const generateQR = async () => {
		if (/* title === '' */ false) {
			error = true;
			messageError = ', Title is empty';
			return;
		} else if (promptPayCode === '') {
			error = true;
			messageError = ', PromptPay Code is empty';
			return;
		} else if (price === undefined) {
			error = true;
			messageError = ', Price is empty';
			return;
		}

		error = false;
		const promptPayQR = await promptQR(promptPayCode, { amount: price / members.length });
		amountResult = price / members.length;
		const base64 = await QRCode.toDataURL(promptPayQR);
		//console.log(base64);
		chkGenerate = true;
		const qrImage = document.querySelector('.qrImage') as HTMLDivElement;
		qrImage.innerHTML = `<img src="${base64}" alt="qrcode"/>`;
	};

	const addMember = () => {
		const name = tempName;
		let nameArr: string[];

		if (name == '') return;
		
		nameArr = name.split(',');
		const splitted = nameArr.map((ele) => {
			return ele.trim();
		})

		members = [...members, ...splitted];
		tempName = '';
		if (chkGenerate) {
			generateQR();
		}
	};

	const clearMember = () => {
		members = ['Me'];
		if (chkGenerate) {
			generateQR();
		}
	};

	const removeMember = (index: number) => {
		members.splice(index, 1);
		members = [...members];
		if (chkGenerate) {
			generateQR();
		}
	};
</script>

<svelte:head>
	<title>YouDontPayAlone</title>
	<meta name="title" content="YouDontPayAlone" />
	<meta name="description" content="You are not alone." />
	<meta
		name="viewport"
		content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no"
	/>
</svelte:head>

<main class="p-10 select-none">
	<div class="h-1/2 flex flex-col items-center justify-center gap-3">
		<h1 class="text-5xl">YouDontPayAlone</h1>
		<div class="qrImage flex m-10 self-center" />
		<!-- <input
			bind:value={title}
			type="text"
			class="input input-primary input-bordered md:w-1/2 w-full"
			placeholder="Title"
		/> -->
		<input
			bind:value={promptPayCode}
			type="text"
			class="input input-primary input-bordered md:w-1/2 w-full"
			placeholder="PromptPay Code"/>
		<input
			bind:value={price}
			type="text"
			class="input input-primary input-bordered md:w-1/2 w-full"
			placeholder="Price"
		/>
		{#if error}
			<div class="alert alert-error">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="stroke-current shrink-0 h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
					><path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
					/></svg
				>
				<span>Error!{messageError}.</span>
			</div>
		{/if}
		<button class="btn btn-info" on:click={generateQR}>GENERATE</button>

		<div class="flex flex-row mt-10">
			<div class="flex flex-col w-full gap-3">
				<div class="flex flex-col justify-center gap-1">
					<form>
						<input
							bind:value={tempName}
							type="text"
							class="input input-primary input-bordered"
							placeholder="name, ..."
						/>
						<button class="btn btn-info w-auto max-sm:hidden" on:click={addMember}>Add</button>
					</form>
					<button class="btn btn-info w-auto sm:hidden" on:click={addMember}>Add</button>
					<button class="btn btn-error w-auto" on:click={clearMember}>Clear</button>
				</div>
				<div class="flex justify-end">
					<Members memberCount={members} />
				</div>
				{#each members as member, index}
					<div class="flex flex-row">
						<div class="grid h-8 rounded-sm border-dashed border-2 border-primary w-10/12">
							<div class="flex">
								<div class="flex flex-col w-2/4 mx-2">{member}</div>
								<!-- svelte-ignore a11y-click-events-have-key-events -->
								<!-- svelte-ignore a11y-no-static-element-interactions -->
								<div
									class="flex flex-col w-2/4 mx-2 hover:cursor-pointer text-error"
									on:click={() => removeMember(index)}
								>
									<span class="self-end">
										{amountResult !== undefined ? amountResult.toFixed(2) : 0}$
									</span>
								</div>
							</div>
						</div>
						{#if member !== 'Me'}
							<!-- svelte-ignore a11y-click-events-have-key-events -->
							<!-- svelte-ignore a11y-no-static-element-interactions -->
							<div class="flex flex-col w-2/12" on:click={() => removeMember(index)}>
								<span class="flex self-center text-error hover:cursor-pointer"> del </span>
							</div>
						{:else}
							<div class="flex flex-col w-2/12">
								<span class="flex self-center text-stone-600">-</span>
							</div>
						{/if}
					</div>
				{/each}
			</div>
		</div>
	</div>
</main>
