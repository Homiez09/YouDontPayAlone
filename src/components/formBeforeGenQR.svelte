<script lang="ts">
    export let getURLeSearchParams;

    import Members from '../components/members.svelte';

	let title: string = '';
	let price: number;

	let members: string[];
	let tempName: string = '';
	let promptPayCode: string = '';
	let error: boolean = false;
	let messageError: string;

    title = getURLeSearchParams.title;
    promptPayCode = getURLeSearchParams.promptPayCode;
    price = getURLeSearchParams.price;
    members = getURLeSearchParams.members;

	const generateQR = async () => {
		if (title === '') {
			error = true;
			messageError = ', Title is empty';
			return;
		} else if (promptPayCode === '') {
			error = true;
			messageError = ', PromptPay Code is empty';
			return;
		} else if (price <= 0) {
			error = true;
			messageError = ', Price is not correct';
			return;
		} else {
            error = false;
        }

        const url = new URLSearchParams();
        url.set('title', title)
        url.set('promptPayCode', promptPayCode);
        url.set('price', price.toString());
        url.set('members', members.join(','));
        history.pushState({}, '', `/?${url.toString()}`);

        window.location.href = `/generate?title=${title}&promptPayCode=${promptPayCode}&price=${price}&members=${members.join(',')}`;
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
	};

	const clearMember = () => {
		members = [];
	};

	const removeMember = (index: number) => {
		members.splice(index, 1);
		members = [...members];
	};
</script>

<div class="qrImage flex m-10 self-center" />
<input
    bind:value={title}
    type="text"
    class="input {title == '' ? 'input-error' : 'input-primary'} input-bordered md:w-1/2 w-full"
    placeholder="Title"
/>
<input
    bind:value={promptPayCode}
    type="text"
    class="input {promptPayCode == '' ? 'input-error' : 'input-primary'} input-bordered md:w-1/2 w-full"
    placeholder="PromptPay Code"/>
<input
    bind:value={price}
    type="text"
    class="input {price > 0 ? 'input-primary' : 'input-error'} input-bordered md:w-1/2 w-full"
    placeholder="Price"
/>

<div class="flex flex-row md:w-1/2 w-full">
    <div class="flex flex-col w-full gap-4">
        <div class="flex justify-end">
            <Members memberCount={members} />
        </div>
        <div class="flex flex-row justify-center gap-2 items-end"> 
            <input
                bind:value={tempName}
                type="text"
                class="input input-primary input-bordered w-full"
                placeholder="John, Jane, Doe, ..."
            />
            <button class="btn btn-md btn-info w-auto font-700" on:click={addMember}>add</button>
            <button class="btn btn-error w-auto" on:click={clearMember}>clear</button>
        </div>
        {#each members as member, index}
            <div class="flex flex-row">
                <div class="grid h-8 rounded-sm border-dashed border-2 border-primary w-11/12">
                    <div class="flex">
                        <div class="flex flex-col w-1/2 mx-2 gap-5">{member}</div>
                        <div
                            class="flex flex-col w-1/2 mx-2 text-error"
                        >
                            <span class="self-end">
                                {(price / members.length).toFixed(2)}$
                            </span>
                        </div>
                    </div>
                </div>
                {#if member !== 'Me'}
                    <!-- svelte-ignore a11y-click-events-have-key-events -->
                    <!-- svelte-ignore a11y-no-static-element-interactions -->
                    <div class="flex flex-col w-1/12" on:click={() => removeMember(index)}>
                        <span class="flex self-center text-error hover:cursor-pointer"> del </span>
                    </div>
                {:else}
                    <div class="flex flex-col w-1/12">
                        <span class="flex self-center text-stone-600">-</span>
                    </div>
                {/if}
            </div>
        {/each}
    </div>
</div>

{#if error}
    <div class="alert alert-error md:w-1/2 w-full">
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

<button class="my-7 btn btn-info w-1/3" on:click={generateQR}>GENERATE</button>