<script lang="ts">
	import Members from '../components/members.svelte';
	import promptQR from 'promptpay-qr';
	import QRCode from 'qrcode';

	let title: string = '';
	let price: number;
	let members: string[] = ['Me'];
	let tempName: string = '';
	let promptPayCode: string = '';
	let error: boolean = false;
	let chkGenerate: boolean = false;

	const generateQR = async () => {
		if (title === '' || price === undefined || promptPayCode === '') {
			error = true;
			return;
		}
        error = false;
		const promptPayQR = await promptQR(promptPayCode, { amount: price / members.length });
		const base64 = await QRCode.toDataURL(promptPayQR);
		console.log(base64);
		chkGenerate = true;
		const qrImage = document.querySelector('.qrImage') as HTMLDivElement;
		qrImage.innerHTML = `<img src="${base64}" alt="qrcode"/>`;
	};

	const addMember = () => {
		const name = tempName;
		if (name == '') return;
		members = [...members, name];
		tempName = '';
        if (chkGenerate) {  
            generateQR();
        }
	};

    const clearMember = () => {
       members = ['Me']
    }
</script>

<main class="p-10">
    <div class="qrImage flex m-10" />
	<div class="h-1/2 flex flex-col items-center justify-center gap-3">
		<input
			bind:value={title}
			type="text"
			class="input input-primary input-bordered md:w-1/2 w-full"
			placeholder="Title"
		/>
		<input
			bind:value={promptPayCode}
			type="text"
			class="input input-primary input-bordered md:w-1/2 w-full"
			placeholder="PromptPay Code"
		/>
		<input
			bind:value={price}
			type="number"
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
				<span>Error! failed successfully.</span>
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
                            placeholder="Name"
                        />
                        <button class="btn btn-info w-auto" on:click={addMember}>Add</button>
                    </form>
                    <button class="btn btn-error w-auto" on:click={clearMember}>Clear</button>
                </div>
                <div class="flex justify-end">
                    <Members memberCount={members}/>
                </div>
            </div>
        </div>
	</div>
</main>
